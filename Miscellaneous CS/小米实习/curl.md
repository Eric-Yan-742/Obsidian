[https://blog.csdn.net/byxdaz/article/details/113363501](https://blog.csdn.net/byxdaz/article/details/113363501)

curl 跟 Postman 这种软件的作用其实差不多，用来调试的。

例子：

```Shell
curl 'https://work-be.test.mi.com/mtop/nr/commission/settlement/queryBillById' \
  -H 'accept: application/json, text/plain, */*' \
  -H 'accept-language: zh-CN,zh;q=0.9' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -H 'cookie: mstuid=1703818215522_5937; Hm_lvt_4982d57ea12df95a2b24715fb6440726=1703818216; xmuuid=XMGUEST-A87CF180-A627-11EE-ABB3-81EB527149DD; xmUuid=XMGUEST-A87CF180-A627-11EE-ABB3-81EB527149DD; _mode_upc_nr_token_dev=cas; upc_nr_token=c9ea7f4d00eab2c42767ccf4bda15efb5859fb8e06c49de1201330340e3a7603; _mode_upc_nr_token=cas; Hm_lvt_c3e3e8b3ea48955284516b186acf0f4e=1721646774,1722940390; upc_nr_token_dev=c9ea7f4d00eab2c42767ccf4bda15efb2be046dade766fae201330340e3a7603' \
  -H 'origin: https://work-be.test.mi.com' \
  -H 'priority: u=1, i' \
  -H 'referer: https://work-be.test.mi.com/main-incentives/carIncentive/entry/billDetail?settlementId=QC202408T6&canEntry=0' \
  -H 'sec-ch-ua: "Google Chrome";v="125", "Chromium";v="125", "Not.A/Brand";v="24"' \
  -H 'sec-ch-ua-mobile: ?0' \
  -H 'sec-ch-ua-platform: "Windows"' \
  -H 'sec-fetch-dest: empty' \
  -H 'sec-fetch-mode: cors' \
  -H 'sec-fetch-site: same-origin' \
  -H 'user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/125.0.0.0 Safari/537.36' \
  -H 'x-requested-with: XMLHttpRequest' \
  --data-raw '[{"settlementId":"QC202408T6","bizType":1}]'
```

第一行的是url，后面-H跟的都是请求头，最后一行 —data-row 后面跟的是数据，同时说明默认是一个POST请求。  
如果没有使用  
`-d`（或 `--data-raw`），也没有使用 `-X` 指定请求方法，`curl` 默认发送 `GET` 请求。