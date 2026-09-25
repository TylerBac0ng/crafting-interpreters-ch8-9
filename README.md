# Crafting Interpreters — Chapters 8 & 9 Challenges

Solutions to the end-of-chapter challenges from [_Crafting Interpreters_](https://craftinginterpreters.com/) by Robert Nystrom, chapters **8 (Statements and State)** and **9 (Control Flow)**. This is the complete jlox tree-walk interpreter through chapter 9, so it compiles and runs on its own.

## Build and run

```sh
javac -d build $(find src -name '*.java')
java -cp build com.craftinginterpreters.lox.Lox            # REPL
java -cp build com.craftinginterpreters.lox.Lox file.lox   # run a script
./run_tests.sh                                            # run the tests
```

## Chapter 8 — Statements and State

1. **REPL expressions and statements.** Type a bare expression with no
   semicolon (`1 + 2`) and the REPL prints its value. Statements still run
   as usual. See `Parser.parseRepl()` and `Lox.runRepl()`.
2. **Uninitialized variables are an error.** `var a; print a;` is now a
   runtime error. A variable that was explicitly set to `nil` still works.
   The interpreter stores a private `uninitialized` sentinel for
   `var a;` and checks for it in `visitVariableExpr()`.
3. **`var a = a + 2;` in a nested block.** Written answer only (see
   `answers.pdf`). `tests/ch8_shadow.lox` shows how jlox behaves.

## Chapter 9 — Control Flow

1. **Branching without `if`.** Written answer (see `answers.pdf`).
2. **Looping without `while`/`for`.** Written answer (see `answers.pdf`).
3. **`break` statement.** Adds the `BREAK` token, the `Stmt.Break` node,
   and parsing that tracks loop depth, so `break` outside a loop is a
   syntax error. At run time, `break` throws a `BreakException` that the
   nearest `while` catches. A `for` loop is turned into a `while` loop, so
   it works there too.

## Files changed from the book's chapter 9 code

| File | Change |
|---|---|
| `Lox.java` | `runRepl()` runs statements or prints a bare expression's value (8.1) |
| `Parser.java` | `parseRepl()` / `allowExpression` (8.1); `breakStatement()` and `loopDepth` (9.3) |
| `Interpreter.java` | `interpret(Expr)` (8.1); `uninitialized` sentinel (8.2); `visitBreakStmt` / `BreakException` (9.3) |
| `TokenType.java`, `Scanner.java` | `BREAK` keyword (9.3) |
| `tool/GenerateAst.java` → `Stmt.java` | `Break : Token keyword` node (9.3) |
