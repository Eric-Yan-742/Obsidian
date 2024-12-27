```Java
// 校验excel数据是否包含含空字段
List<BonusManualExcelEntity> excelEntityList;
try {
    excelEntityList = excelGateway.importExcel(billEntity.getExt().getUrl(), BonusManualExcelEntity.class);
} catch (Exception e) {
    log.error("parseExcel error.", e);
    throw new BusinessException(ErrorCodeEnums.INTERNAL_ABNORMAL_ERROR, "文件解析异常");
}
List<BonusManualExcelEntity> emptyColumnList = excelEntityList.stream().filter(BonusManualExcelEntity::isAnyFieldEmpty).collect(Collectors.toList());
if (CollectionUtils.isNotEmpty(emptyColumnList)) {
    log.error("parseExcel contain emptyColumnList.emptyColumnList={}", emptyColumnList);
    throw new BusinessException(ErrorCodeEnums.VALIDATE_ERROR, "上传内容包含空字段");
}


// 解析excel数据
List<SettlementDetailEntity> detailEntityList = excelEntityList.stream().map(v -> {
            BigDecimal excelAmount = new BigDecimal(v.getBonusAmount());
            BigDecimal serviceAmount = excelAmount.multiply(servicePointGateway.getServicePoint()).setScale(Constants.DECIMAL_PLACES_NUM, RoundingMode.HALF_UP);
            BigDecimal bonusAmount = excelAmount.multiply(Constants.BIG_DECIMAL_HUNDRED).setScale(Constants.DECIMAL_PLACES_NUM, RoundingMode.HALF_UP);
            BigDecimal sumAmount = bonusAmount.add(serviceAmount);
            BonusSettlementDetailExt detailExt = BonusSettlementDetailExt.builder()
                    .settlementMonth(billEntity.getSettlementMonth())
                    .awardeeName(v.getAwardeeName())
                    .positionName(v.getPositionName())
                    // 单位由元转分
                    .serviceAmount(serviceAmount)
                    .sumAmount(sumAmount)
                    .build();

            return SettlementDetailEntity.builder()
                    .awardeeId(v.getAwardeeId())
                    .settlementId(billEntity.getSettlementId())
                    .policyId(billEntity.getPolicyId())
                    .policyType(PolicyTypeEnum.getByDesc(v.getBonusType()).getCode())
                    .awardAmount(bonusAmount)
                    .serviceAmount(detailExt.getServiceAmount())
                    .positionName(detailExt.getPositionName())
                    .ext(GsonUtil.toJson(detailExt))
                    .build();
        }).collect(Collectors.toList());

// 检验解析后的数据不能为空
log.info("parseExcel data size={}", detailEntityList.size());
if (CollectionUtils.isEmpty(detailEntityList)) {
    log.error("handleUploadDetails detailEntityList is empty.");
    throw new BusinessException(ErrorCodeEnums.VALIDATE_ERROR, "上传的明细数据为空");
}
```