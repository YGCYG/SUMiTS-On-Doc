# Separator

![Sample Configuration (1/2)](<../../../../.gitbook/assets/image (9).png>)

![Sample Configuration (2/2)](<../../../../.gitbook/assets/image (8).png>)

#### Sample Output

{% code overflow="wrap" lineNumbers="true" %}
```json
{
  "id": "parser",
  "kind": "",
  "key": "",
  "capture": "(.*)",
  "tokenizer": "sep",
  "tokenizeRule": [
    "[|]"
  ],
  "assign": [
    [
      "1",
      "2020-03-19T04",
      ""
    ],
    [
      "2",
      "26:23Z",
      ""
    ],
    [
      "3",
      "%FTD-0-430001",
      ""
    ],
    [
      "4",
      "DeviceUUID",
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
