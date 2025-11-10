# Tactic and Technique group counts splunk query
### This query help to visualize the TTP's and their counts in each Tactic

```| inputlookup for-spss-ingest.csv
| stats count(Tactics) as Tactics_count by ttpID, Tactics
| stats list(*) as * by Tactics
| appendcols
    [ 
| inputlookup for-spss-ingest.csv
| stats sum(ttpID) as ttpID count as ttpTotal by Tactics
    | addcoltotals ttpTotal label=Total labelfield=Total
     ]
```
### Resulting Analytic Table

| Tactics              | Tactics_count | Total | ttpID | ttpTotal |
| -------------------- | ------------- | ----- | ----- | -------- |
| Collection           | 7             |       | T1005 | 11       |
|                      | 4             |       | T1114 |          |
| Command and Control  | 4             |       | T1071 | 11       |
|                      | 7             |       | T1105 |          |
| Credential Access    | 7             |       | T1003 | 7        |
| Defense Evasion      | 11            |       | T1027 | 66       |
|                      | 8             |       | T1036 |          |
|                      | 4             |       | T1055 |          |
|                      | 7             |       | T1070 |          |
|                      | 2             |       | T1078 |          |
|                      | 6             |       | T1112 |          |
|                      | 4             |       | T1134 |          |
|                      | 11            |       | T1140 |          |
|                      | 2             |       | T1203 |          |
|                      | 2             |       | T1550 |          |
|                      | 6             |       | T1562 |          |
|                      | 3             |       | T1574 |          |
| Discovery            | 6             |       | T1016 | 30       |
|                      | 9             |       | T1082 |          |
|                      | 8             |       | T1083 |          |
|                      | 7             |       | T1087 |          |
| Execution            | 13            |       | T1059 | 34       |
|                      | 1             |       | T1068 |          |
|                      | 14            |       | T1203 |          |
|                      | 6             |       | T1569 |          |
| Exfiltration         | 9             |       | T1041 | 9        |
| Impact               | 10            |       | T1486 | 10       |
| Initial Access       | 2             |       | T1078 | 60       |
|                      | 9             |       | T1133 |          |
|                      | 36            |       | T1190 |          |
|                      | 4             |       | T1195 |          |
|                      | 9             |       | T1566 |          |
| Lateral Movement     | 10            |       | T1021 | 20       |
|                      | 8             |       | T1210 |          |
|                      | 2             |       | T1550 |          |
| Persistence          | 2             |       | T1078 | 14       |
|                      | 9             |       | T1133 |          |
|                      | 3             |       | T1574 |          |
| Privilege Escalation | 4             |       | T1055 | 30       |
|                      | 15            |       | T1068 |          |
|                      | 2             |       | T1078 |          |
|                      | 4             |       | T1134 |          |
|                      | 1             |       | T1203 |          |
|                      | 1             |       | T1499 |          |
|                      | 3             |       | T1574 |          |
|                      |               | Total |       | 302      |


### Additional parameter tacticflow added for Tactic flow Sequence
#### Tacticflow helps assign and visualise the progression of an attack

Splunk SPL search to analyze and display this info

```
| inputlookup for-spss-ingest.csv
| stats count(Tactics) as Tactics_count values(ttppercent) as tacticrisk by ttpID, Tactics
| lookup tacticflow.csv Tactic_Name as Tactics output Sequence as tacticflow 
| stats list(*) as * by Tactics, tacticrisk, tacticflow
| appendcols
    [ 
| inputlookup for-spss-ingest.csv
| stats sum(ttpID) as ttpID count as ttpTotal by Tactics
    | addcoltotals ttpTotal label=Total labelfield=Total
     ]
```
| Tactics              | tacticrisk | tacticflow | Tactics_count | Total | ttpID | ttpTotal |
| -------------------- | ---------- | ---------- | ------------- | ----- | ----- | -------- |
| Collection           | 41.83      | 11         | 7             |       | T1005 | 11       |
|                      |            |            | 4             |       | T1114 |          |
| Command and Control  | 18.88      | 12         | 4             |       | T1071 | 11       |
|                      |            |            | 7             |       | T1105 |          |
| Credential Access    | 13.85      | 8          | 7             |       | T1003 | 7        |
| Defense Evasion      | 27.82      | 7          | 11            |       | T1027 | 66       |
|                      |            |            | 8             |       | T1036 |          |
|                      |            |            | 4             |       | T1055 |          |
|                      |            |            | 7             |       | T1070 |          |
|                      |            |            | 2             |       | T1078 |          |
|                      |            |            | 6             |       | T1112 |          |
|                      |            |            | 4             |       | T1134 |          |
|                      |            |            | 11            |       | T1140 |          |
|                      |            |            | 2             |       | T1203 |          |
|                      |            |            | 2             |       | T1550 |          |
|                      |            |            | 6             |       | T1562 |          |
|                      |            |            | 3             |       | T1574 |          |
| Discovery            | 20.99      | 9          | 6             |       | T1016 | 30       |
|                      |            |            | 9             |       | T1082 |          |
|                      |            |            | 8             |       | T1083 |          |
|                      |            |            | 7             |       | T1087 |          |
| Execution            | 31.18      | 4          | 13            |       | T1059 | 34       |
|                      |            |            | 1             |       | T1068 |          |
|                      |            |            | 14            |       | T1203 |          |
|                      |            |            | 6             |       | T1569 |          |
| Exfiltration         | 41.46      | 13         | 9             |       | T1041 | 9        |
| Impact               | 13.85      | 14         | 10            |       | T1486 | 10       |
| Initial Access       | 41.28      | 3          | 2             |       | T1078 | 60       |
|                      |            |            | 9             |       | T1133 |          |
|                      |            |            | 36            |       | T1190 |          |
|                      |            |            | 4             |       | T1195 |          |
|                      |            |            | 9             |       | T1566 |          |
| Lateral Movement     | 26.55      | 10         | 10            |       | T1021 | 20       |
|                      |            |            | 8             |       | T1210 |          |
|                      |            |            | 2             |       | T1550 |          |
| Persistence          | 42.21      | 5          | 2             |       | T1078 | 14       |
|                      |            |            | 9             |       | T1133 |          |
|                      |            |            | 3             |       | T1574 |          |
| Privilege Escalation | 44.69      | 6          | 4             |       | T1055 | 30       |
|                      |            |            | 15            |       | T1068 |          |
|                      |            |            | 2             |       | T1078 |          |
|                      |            |            | 4             |       | T1134 |          |
|                      |            |            | 1             |       | T1203 |          |
|                      |            |            | 1             |       | T1499 |          |
|                      |            |            | 3             |       | T1574 |          |
|                      |            |            |               | Total |       | 302      |
