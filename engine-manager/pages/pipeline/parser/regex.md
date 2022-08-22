# Regex

![Sample Configuration (1/2)](<../../../../.gitbook/assets/image (4).png>)

![Sample Configuration (2/2)](<../../../../.gitbook/assets/image (2).png>)

#### Sample Output

{% code overflow="wrap" lineNumbers="true" %}
```json
{
  "id": "parser",
  "kind": "",
  "key": "",
  "capture": "(.*)",
  "tokenizer": "reg",
  "tokenizeRule": [
    "^(\\d{4}-\\d{2}-\\d{2}T\\d{2}:\\d{2}:\\d{2})Z\\s+(%FTD-\\d+-430001):\\s(DeviceUUID:[^,]*)"
  ],
  "assign": [
    [
      "1",
      "2020-03-19T04:26:23",
      ""
    ],
    [
      "2",
      "%FTD-0-430001",
      ""
    ],
    [
      "3",
      "DeviceUUID: 9be58130-b577-11e9-9be3-ff86904f11c1",
      ""
    ]
  ],
  "ref": [
    {
      "seq": "1",
      "tableName": "test3",
      "lookup": [
        "test=test"
      ]
    }
  ]
}
```
{% endcode %}
