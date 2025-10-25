## First Sets

| Non-Terminal | Round 1 | Round 2 | Round 3   | Round 4       | Round 5       |
| ------------ | ------- | ------- | --------- | ------------- | ------------- |
| S            | {}      | {a}     | {a,c,ɛ}   | {c,b,a,ɛ}     | {c,b,a,ɛ}     |
| A            | {}      | {ɛ}     | {ɛ,c,a}   | {ɛ,c,a,b,g,e} | {ɛ,c,b,a,g,e} |
| B            | {}      | {a}     | {a,e,g,ɛ} | {a,e,g,ɛ,b,c} | {a,e,g,ɛ,b,c} |
| C            | {}      | {c,ɛ}   | {c,ɛ}     | {c,ɛ}         | {c,ɛ}         |
| D            | {}      | {ɛ}     | {c,b,ɛ}   | {c,b,ɛ}       | {c,b,ɛ}       |
| E            | {}      | {e}     | {e}       | {e}           | {e}           |
| F            | {}      | {ɛ,g}   | {ɛ,g}     | {g,ɛ}         | {g,ɛ}         |
FIRST(S)->{c,b,a,ɛ}    
FIRST(A)->{ɛ,c,b,a,g,e}
FIRST(B)->{a,e,g,ɛ,b,c}
FIRST(C)->{c,ɛ}        
FIRST(D)->{c,b,ɛ}      
FIRST(D)->{e}          
FIRST(E)->{g,ɛ}        

## Follow Sets
| Non-Terminal | Round 1 | Round 2       | Round 3       |
| ------------ | ------- | ------------- | ------------- |
| S            | {}      | {$}           | {$}           |
| A            | {}      | {a,b,c,g,e}   | {a,b,c,g,e}   |
| B            | {}      | {c,e}         | {c,e}         |
| C            | {}      | {$,a,b,c,g,e} | {$,a,b,c,g,e} |
| D            | {}      | {$,c,b,a,g,e} | {$,c,b,a,g,e} |
| E            | {}      | {a,b,c,g,e}   | {a,b,c,g,e}   |
| F            | {}      | {c,e,g}       | {c,e,g}       |
Follow(S)->{\$}          
Follow(A)->{a,b,c,g,e}  
Follow(B)->{c,e}        
Follow(C)->{\$,a,b,c,g,e}
Follow(D)->{\$,c,b,a,g,e}
Follow(E)->{a,b,c,g,e}  
Follow(F)->{c,e,g}      