- Java Stream map和flatmap
    
    - map将流中的一个对象转换成另一个对象。如何转换，转换结果的类型都由 lambda 表达式自定义。
    - flatMap将流中的一个对象转换成多个对象，与map类似，lambda表达式决定转换的过程与结果类型，不同点在于转换结果必须为一个stream，所以需要多一步转换成stream。
    
    ```Java
    private static List<BonusSummary> getBonusSummaryList(List<SettlementDetailEntity> detailEntityList) {
        Map<Integer, Map<String, BonusSummary>> bonusSummaryMap = detailEntityList.stream()
    		    // 按policyType分组
    		    .collect(Collectors.groupingBy(SettlementDetailEntity::getPolicyType, 
    		    // 按刚才的分组，继续按positionName分组
    		    Collectors.groupingBy(SettlementDetailEntity::getPositionName, 
    		    // 将二重分组的结果重新收集成list并进行操作
    		    Collectors.collectingAndThen(
            Collectors.toList(),
            list -> {
                long awardeeCount = list.stream().map(SettlementDetailEntity::getAwardeeId)
    		            .distinct().count();
    
                BigDecimal amount = list.stream()
                        .map(SettlementDetailEntity::getAwardAmount)
                        .reduce(BigDecimal.ZERO, BigDecimal::add);
    
                BigDecimal serviceAmount = list.stream()
                        .map(SettlementDetailEntity::getServiceAmount)
                        .reduce(BigDecimal.ZERO, BigDecimal::add);
    
                BigDecimal totalAmount = amount.add(serviceAmount);
    
                // 获取 policyType 和 positionName
                Integer policyType = list.get(0).getPolicyType();
                String positionName = list.get(0).getPositionName();
    
                return BonusSummary.builder().awardeeCount((int) awardeeCount)
                        .amount(amount)
                        .serviceAmount(serviceAmount)
                        .totalAmount(totalAmount)
                        .policyType(policyType)
                        .positionName(positionName)
                        .build();
            }
        ))));
    
        return bonusSummaryMap.values().stream()
                .flatMap(positionMap -> positionMap.values().stream())
                .collect(Collectors.toList());
    }
    ```
    
- 根据多个字段进行分组的另一种方法，更方便
    
    ```Java
    Map<String, List<BonusManualExcelExportEntity>> keyEntityMap = exportEntityList.stream().collect(Collectors.groupingBy(e -> e.getAwardeeId() + e.getBonusType() + e.getPositionName()));
    for (List<BonusManualExcelExportEntity> entityList : keyEntityMap.values()) {
        if (entityList.size() == 1) {
            continue;
        }
        for (BonusManualExcelExportEntity excelExportEntity : entityList) {
            if (Strings.isNullOrEmpty(excelExportEntity.getErrorMsg())) {
                parseEntity.setErrorNum(parseEntity.getErrorNum() + 1);
                excelExportEntity.setErrorMsg("核算月份+人+岗+激励类型明细存在重复行");
            } else {
                excelExportEntity.setErrorMsg(excelExportEntity.getErrorMsg() + ";核算月份+人+岗+激励类型明细存在重复行");
            }
        }
    }
    ```