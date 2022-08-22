# JSON

![Sample Configuration (1/2)](<../../../../.gitbook/assets/image (11).png>)

![Sample Configuration (2/2)](<../../../../.gitbook/assets/image (5).png>)

#### Sample Output

{% code overflow="wrap" lineNumbers="true" %}
```json
{
  "id": "parser",
  "kind": "",
  "key": "",
  "capture": "(.*)",
  "tokenizer": "json",
  "tokenizeRule": [
    "employees/employee/firstName(1)",
    "employees/employee(1)"
  ],
  "assign": [
    [
      "employees/employee/firstName(1)",
      "Maria",
      ""
    ],
    [
      "employees/employee(1)",
      "Map(id -> 2, firstName -> Maria, lastName -> Sharapova, photo -> https://jsonformatter.org/img/Maria-Sharapova.jpg)",
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
