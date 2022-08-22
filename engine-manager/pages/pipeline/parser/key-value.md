# Key-Value

![Sample Configuration (1/2)](<../../../../.gitbook/assets/image (12).png>)

![Sample Configuration (2/2)](<../../../../.gitbook/assets/image (13).png>)

#### Sample Output

{% code overflow="wrap" lineNumbers="true" %}
```json
{
  "id": "parser",
  "kind": "",
  "key": "",
  "capture": "(.*)",
  "tokenizer": "kv",
  "tokenizeRule": [
    "[|]::[=]"
  ],
  "assign": [
    [
      "date",
      "2020-03-19T04",
      ""
    ],
    [
      "time",
      "26:23Z",
      ""
    ],
    [
      "id",
      "%FTD-0-430001",
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
