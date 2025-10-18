Token is some abstraction of a character or word ect... typically a->z | A->Z | 0->9 or special character.
e = epsolon = empty string
e s = s, e = s
keen star, or asterisc, *  is a way to define as many matches as available
What is an regular expression
1. null
2. e
3. a, where a is a element of the alphabet - a token
4. R1 | R2 where R1 and R2 are regular expressions
5. R1 . R2 where R1 and R2 are regular expressions
6. (R) where r is a regular expression
7. R* where R is a regular expression

A  = {aa,b} B = {a,b}
A . B = {aaa, aab, ba, bb }
however if
A = {aa,b,e}, B = {a,b}
A . B = {aaa, aab, ba,bb,a,b}
Because A contains an empty string, it is optional to include it when concat with B

Order of operations:
- Escapes
- Quantifiers
- Anchors
- Concat
- Or
(rule1) . (rule2) | (rule3)*
in this case rule 1, then rule 2, then rule 3. despite * having a higher operation than everything else it only applies to the rule that it was applied to. due to the simplicity of this example it would be the same as writing (rule3*)
Regex returns rules based on the longest match first when determining types of returns.
`
letter     =  a | b | c | d
LETTER =  A | B | C | D
digit       =  4 | 5 | 6 | 7 | 9

R1         =  (LETTER |  digit  |  !) • (letter • digit)\* • (letter |  digit)
R2         =  (digit |  ?  |  !) • (letter\* • digit\*)\* • (LETTER | letter)
R3         =  (LETTER | digit | ?) • (letter | digit)\* • (LETTER | digit)
R4         =  (digit • letter | ? | !) • (letter\* • digit\*) • (LETTER)
INPUT Aa5a5A5555a5555E
`
In this case it will result in R3, R3, error. as R3 is longer at the first step than R1, it will execute. Then the only match left is R3. Then it will have an unmatched remainder of E

1.1abc1.2

| Input | Match  | potential | longest   | output      |
| ----- | ------ | --------- | --------- | ----------- |
| 1     | digit  | num,dec   | pdiget(1) |             |
| 1.    | -      | dec       | pdiget(1) |             |
| 1.1   | dec    | dec       | dec(3)    |             |
| 1.1a  | -      | -         | -         | dec, 1.1    |
| a     | letter | letter    | letter(1) |             |
| ab    | letter | letter    | letter(2) |             |
| abc   | letter | letter    | letter(3) |             |
| abc1  | -      | -         | -         | letter, abc |
| 1     | digit  | num,dec   | digit(1)  |             |
| 1.    | -      | dec       | digit(1)  |             |
| 1.1   | dec    | dec       | dec(3)    |             |
| eof   |        |           |           | dec,1.1     |
