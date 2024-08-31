[Backus–Naur form - Wikiwand](https://www.wikiwand.com/en/Backus%E2%80%93Naur_form)

## 1	是什么 

BNF can be described as a [metasyntax](https://www.wikiwand.com/en/Metasyntax "Metasyntax") notation for [context-free grammars](https://www.wikiwand.com/en/Context-free_grammar "Context-free grammar").

## 2	做什么

BNFs describe how to combine different symbols to produce a syntactically correct sequence. 

BNFs consist of three components: 
1. a set of non-terminal symbols
2. a set of terminal symbols
3. rules for replacing non-terminal symbols with a sequence of symbols.

These so-called "derivation rules" are written as

	<symbol> ::= __expression__
<!--ID: 1721204088737-->

	
- <[symbol](https://www.wikiwand.com/en/Symbol "Symbol")>[](https://www.wikiwand.com/en/Backus%E2%80%93Naur_form#cite_note-class-2) is a _[nonterminal](https://www.wikiwand.com/en/Nonterminal "Nonterminal")_ variable that is always enclosed between the pair <>.
- `::=` means that the symbol on the left must be replaced with the expression on the right.
<!--ID: 1721204088739-->

- [__expression__](https://www.wikiwand.com/en/Expression_(mathematics) "Expression (mathematics)") consists of one or more sequences of either terminal or nonterminal symbols where each sequence is separated by a [vertical bar](https://www.wikiwand.com/en/Vertical_bar "Vertical bar") "|" indicating a [choice](https://www.wikiwand.com/en/Alternation_(formal_language_theory) "Alternation (formal language theory)"), the whole being a possible substitution for the symbol on the left.
