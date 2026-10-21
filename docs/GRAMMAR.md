# BASM Grammar (Initial EBNF)

```ebnf
program      = { declaration } ;
declaration  = module | import | class | struct | interface | function | asm_function | global ;
module       = "module" qualified_name ;
import       = "import" qualified_name ;
function     = "function" identifier "(" [ parameters ] ")" [ ":" type ] block "end" ;
asm_function = "asm_function" identifier "(" [ parameters ] ")" [ ":" type ] asm_block "end" ;
class        = "class" identifier [ "extends" identifier ] { class_member } "end" ;
struct       = "struct" identifier { field } "end" ;
interface    = "interface" identifier { method_signature } "end" ;
class_member = field | constructor | function ;
field        = "field" identifier ":" type ;
constructor  = "constructor" "(" [ parameters ] ")" block "end" ;
block        = { statement } ;
statement    = local_decl | assignment | if_stmt | while_stmt | for_stmt | return_stmt | expression | asm_block ;
local_decl   = "local" identifier [ ":" type ] [ "=" expression ] ;
if_stmt      = "if" expression "then" block { "elseif" expression "then" block } [ "else" block ] "end" ;
while_stmt   = "while" expression "do" block "end" ;
for_stmt     = "for" identifier "=" expression "," expression "do" block "end" ;
return_stmt  = "return" [ expression ] ;
asm_block    = "asm" { asm_line } "end" ;
parameters   = parameter { "," parameter } ;
parameter    = identifier ":" type ;
type         = identifier [ "<" type ">" ] ;
qualified_name = identifier { "." identifier } ;
identifier   = letter { letter | digit | "_" } ;
```

This grammar is intentionally small. Expression precedence, pattern matching, closures, decorators, and macro syntax will be specified separately as the parser matures.
