
Example:


| #   | L1     | Transactions      |
| --- | ------ | ----------------- |
| 1   | Beer   | T3, T4            |
| 2   | Bread  | T0,T1,T2,T3,T4,T5 |
| 3   | Butter | T0,T1,T3,T4,T5    |
| 4   | Diaper | T2,T4,T5          |
| 5   | Milk   | T0,T2,T4,T5       |

| #   | L2                | Transactions   |
| --- | ----------------- | -------------- |
| 1   | Bread,Butter      | T0,T1,T3,T4,T5 |
| 2   | Bread,Diaper      | T2,T4,T5       |
| 3   | Bread,Milk        | T0,T2,T4,T5    |
| 4   | ~~Butter,Diaper~~ | ~~T4,T5~~      |
| 5   | Butter,Milk       | T0,T4,T5       |
| 6   | Diaper,Milk       | T2,T4,T5       |
