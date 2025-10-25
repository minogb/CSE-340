Regex are not enough to create a language. Regex is great for generating tokens. Because regex has no memory it can only tell that the tokens are in an expected for and not if the tokens are in an expected order.
ie ; int j and int j; are both valid for regex and need something more advanced.
# Context-Free Grammars
each input row is called a production
- Non terminals on the left
- right arrow
- non terminals and terminals on the right
Non-terminals will start with an upper case and terminals will be lower case and are tokens
S = starting nonterminal
Non terminals are named as such because they are a named set of both terminals and nonterminals
for example
S => (S) defines S as a non terminal that starts with ( and then has an S non terminal and ends with ) terminal
you can also have multiple production rules for the same terminal such that
S=> e
S=>(S)
defines S as either an empty string or an S within ()
(((S))) is a valid example for this rule

E => E + E
E => E * E
E => NUM
defines a basic chain of additions and multiplications
# Left Most vs Right Most
left most - always expands the left most non terminal first
so
E=> E1 * E2 => 1 * E2 => 1 * E3 + E4 => 1 * 2 + E4 => 1 * 2 + 3
and right most just expands the right most non terminal first
# Parse tree
a parser always follows a lexer which is the thing that generates the tokens using regex.
A parse tree is a representation of actions taken by doing a left most or right most action. in the example bellow this was done with a right most parse.
![[Pasted image 20251018112344.png]]
# Predictive Recursive Descent Parser
- are efficient top down parsers - no backtracking
- FIRST(alpha) where alpha is a sequence of grammar symbols - returns the set of terminals and e that begin strings derived from alpha
- FOLLOW(A) where A is a non-terminal - returns the set of terminals and $ (eof) that can appear immediately after A
if alpha = ABaCd
then the first(alpha) = a set of the first of the set for each of the strings it derives.
so if the available sets it generats are {ad,efad,facd...} then first(alpha) => {a,e,f}
Follow(B) => {a} as a is the only thing that can follow B
Follow(A)=> First(B)
# First Sets
example:
S=> A|B|C
A=>a
B=>Bb|b
C=>Cc|e
FIRST(S) =>FIRST(A) U FIRST(B) U FIRST(B) => {a,b,c,e}
FIRST(A) => {a}
FIRST(B) => {b}
FIRST(C) => {e,c}
## Rules
1. FIRST(x) = {x} where x is any terminal
2. FIRST(e) = {e}
3. if A => Ba then add FIRST(B) - {e} to first(A) - because A can never start with e as it must always have a
4. if A => B0,B1,...Bi,Bi+1..., Bk and e is in FIRST(B0) and e in FIRST of B(1).... and e in FIRST(Bi) then add FIRST(Bi+1) - {e} for FIRST (A) - i is any target point after B0 - if you observe e is in all the sets then do the rule. Basically if you are making all the B = e then add the first what ever one that does not have e to the set of FIRST(A)
5. if A => B0,B1,...Bi,Bi+1..., Bk and e in FIRST(B1)... ect... then add e to FIRST(A). AKA if all the B's have e then you can add e to FIRST(A)

### Example
S => ABCD
A=> CD | aA
B => b
C => cC | e
D => dD | e

| RULE     | Round 1 | Round 2 | Round 3   | Round 4   |
| -------- | ------- | ------- | --------- | --------- |
| FIRST(S) | {}      | {}      | {a}       | {a,c,d,b} |
| FIRST(A) | {}      | {a}     | {a,c,d,e} | {a,c,d,e} |
| FIRST(B) | {}      | {b}     | {b}       | {b}       |
| FIRST(C) | {}      | {e,c}   | {e,c}     | {e,c}     |
| FIRST(D) | {}      | {d,e}   | {e,d}     | {e,d}     |

Steps:
- Rule 3 states A => B alpha. Rule S is this way. In this case this would take the FIRST(A), however FIRST(A) is empty at this set.
- Rule 4 also applies but first(a) is empty so first(s) is empty
- Rule 5 also applies but first(a) is empty so first(s) is empty
- Now we do A, we must do both CD and aA rules each
- CD rule has 3-5 as well but they are empty so nothing changes
- Rule 3 applies to aA, in this case the a = B and A = alpha set FIRST(A) gains FIRST(a) - e
- Rule 4 and 5 doesn't apply to FIRST(A) as non of the sub sets include e
- Then for FIRST(C)
- Rule 2 applies e as it has e
- Rule 3 applies so we add FIRST(c) - e
- So first(C) gains e and c
- FIRST(D) has the same steps as first(C)
- Because there was a change in this round of applying rules we have to redo all the rules
- Rule 3 applies now to first(S) (FIRST(A) has an entry). so FIRST(S) gains a
- Rule 4, we apply where i = 0. Because this set is FIRST(A) and it does not have an e, we stop here and apply  nothing
- for FIRST(A) CD we apply rule 3 which means we add first(C) - e to FIRST(A)
- rule 4 FIRST(A) CD we cycle until we hit a rule without e, because we cannot we don't add anything
- rule 5 applies as FIRST(C) and FIRST(D) have e, so we add e
- FIRST(B) doesn't change
- FIRST(C) doesn't change
- FIRST(D) doesn't change
- Because the sets changed, we have to reapply the rules
- applying rule 3 we add FIRST(A) - e
- we can now apply rule 4 to FIRST(S) so we we add FIRST(B) to FIRST(S)
- no other rules change in this round
- Sets have changed so we redo the rules
- because only FIRST(S) changed, no other changes will be done
# Follow Sets
Follow(A) where A is a non-terminal, returns the set of terminals and eof that can appear immediately after the non-terminal A. cannot have e.
S => A | B | C
A=> a
B => Bb | b
C => Cc | e
FOLLOW(S) = {$}
FOLLOW(A) = {$} the only way to get A is from S and since you can only follow with eof it also has eof
FOLLOW(B) = {$, b} B can be arrived from S and B
FOLLOW(C) = {$, c} similar to follow(B)
## Rules
First calculate first sets, then initialize empty follow sets for all non terminals. Finnaly apply the following rules
1. If is the starting symbol of the grammar, then add $ to FOLLOW(S)
2. If B => aA then add FOLLOW(B) to FOLLOW(A). what ever follows B will follow A
3. If B => alpha AC0...Ck and e in FIRST(C0) and e in FIRST(C1).... and e in FIRST(CK) add follow(B) to FOLLOW(A). this basically the same rules as rule 2
4. If B => alphaAC0...Ck then add FIRST(C0) - e to FOLLOW(A).
5. IF B => alphaAC0...CiCi+1...Ck and e in first of any C. then add FIRST(Ci+1) -e to FOLLOW(A)
## Example
S => ABCD
A => CD | aA
B => b
C => cC |e
D => dD | e
FIRST(S) => a,c,d,b
FIRST(A) => a,c,d,e
FIRST(B) => b
FIRST(C) => e,c
FIRST(D) => e,d

| RULE      | Round 1 | Round 2 | Round 3 | Round 4 |
| --------- | ------- | ------- | ------- | ------- |
| FOLLOW(S) | {$}     | {$}     |         |         |
| FOLLOW(A) | {b}     | {b}     |         |         |
| FOLLOW(B) | {$c,d}  | {$c,d}  |         |         |
| FOLLOW(C) | {$,d,b} | {$,d,b} |         |         |
| FOLLOW(D) | {$,d}   | {$,d}   |         |         |
Steps:
- S is starting symbol so it gets eof
- Rule 4 applies FOLLOW(A) so that first(B) is added to FIRST(A) from the prod rule FOLLOW(S)
- Rule two applies to FOLLOW(A), the follow of A has the follow of A, but it doesn't change anything
- Rule 3 applies to S=>ABCD as e is in FIRST(C) and FIRST(D) so FOLLOW(S) is added to FOLLOW(B)
- Rule 4 applies S=>ABCD to add FIRST(C) to FIRST(B)
- Rule 5 applies S=>ABCD to add FIRST(D) to FIRST(B)
- Rule 3 applies to FOLLOW(C) so add FOLLOW(S) to FOLLOW(S)
- Rule 4 applies to FOLLOW(C) so add first(D) to FOLLOW(C)
- Rule 3 applies to FOLLOW(C) as A=>CD and so you add FOLLOW(A) to FOLLOW(C) as whatever comes after A must also come after C since D can be e 
- Rule 2 applies to follow of D from S=>ABCD
- Rule 2 applies to FOLLOW(D) FROM A=>CD
- Round 2
- No additional changes from round one to two. so everything is completed.
# Predictive Recursive Descent Parsers
At each parsing step ther is only one grammar rule that can be chosen and there is no need for back tracking
The conditions for a predictive parser are both of the following:
- if A=> alpha and A => Beta then FIRST(alpha) intersect FIRST(Beta) = null. There should be no first that two different rules can generate.
- If e in FIRST(A) then FIRST(A) intersect FOLLOW(A) = null. If this rule is broken, you are never known if you are done parsing a.
# Creating a Predictive Recursive Descent Parser
- Create a CFG
- Calculate FIRST AND FOLLOW
- Prove that CFG allows Predictive Recursive Descent Parser
- Write the predictive recursive decent parser using the first and follow sets
![[Pasted image 20251023201144.png]]
-----
See example rules for bellow
`
Parse_S(){
	t = get token();
	if(t in first(ABCD)){
		ungettoken();
		parse_A();
		parse_B();
		parse_C();
		parse_D();
		print(s->ABCD)
	}
	else if (t in follow(s)){
		print(eof);
	}
	else{
		print(syntax error)
	}
}
parse_a(){
	t = gettoken()
	if(t in first(cd)){
		ungettoken();
		parse_c()
		parse_d()
		print("A->CD")
	}
	else if(t in FIRST(aA)){
		parse_A()
		print(A->aA)
	}
	else if(t in follow(A)){--
		ungettoken()
		print(A=>E)
	}
	else{
		print syntax error
	}
}
parse_b(){
	t = gettoken()
	if(t in first(b)){
	print(B=>b)
	}
	else{
		print(syntax error)
	}
}
parse_c(){
	t=gettoken()
	if(t in first(cC)){
		parse_c()
		print(cC)
	}
	else if(t in follow(c))
	{
		ungettoken()
		print(c=>e)
	}
	else{
		print(syntax error)
	}
}
`