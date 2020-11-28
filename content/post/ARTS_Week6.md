---
title: "ARTS_Week6"
date: 2020-11-27T15:48:12-08:00
lastmod: 2020-11-27T15:48:12-08:00
draft: false
categories: ["arts"]
---
## Algorithm
Implement a [calculator](https://github.com/gigimushroom/system_programming/blob/master/advance/python/Calculator%20Implementation.ipynb) that supports Parentheses
```python
symbols = ['+', '-', '*', '/']

def tokenizer(str):
    tokens = []
    rhs = ''
    for i in range(len(str)):
        if str[i] == '(' or str[i] == ')' or str[i] in symbols:
            # We got left hand side, save it.
            if rhs:
                tokens.append(rhs)
                rhs = ''
            tokens.append(str[i])
            
        else:
            rhs += str[i]
            
    return tokens

def calc(tokens):
    lhs = tokens[0]
    sym = tokens[1]
    rhs = tokens[2]
    if sym == '+':
        return int(lhs) + int(rhs)
    elif sym == '-':
        return int(lhs) - int(rhs)
    elif sym == '*':
        return int(lhs) * int(rhs)
    elif sym == '/':
        return int(lhs) / int(rhs)
    
    return None

def eval(tokens):
    if len(tokens) == 1:
        return tokens[0]
    
    return parser(tokens)

def parser(tokens):
    # Trim ().
    tokens = tokens[1:-1]
    
    paren_start = False
    for i, t in enumerate(tokens):
        if t == '(':
            paren_start = True
            
        elif t == ')':
            paren_start = False
            # We got LHS in (xxx), next symbol is +/-/*//, add these as tokens
            symbol = tokens[i+1]
            lhs = eval(tokens[:i+1])
            rhs = eval(tokens[i+2:])
            #print(lhs, symbol, rhs)
            tks = [lhs, symbol, rhs]
            return calc(tks)
            
        elif t in symbols and not paren_start:
            symbol = t
            lhs = eval(tokens[:i])
            rhs = eval(tokens[i+1:])
            tks = [lhs, symbol, rhs]
            return calc(tks)
            
    return None
            
print(parser(tokenizer('((2+3)+5)')))
print(parser(tokenizer('(3*(2+4))')))

print(parser(tokenizer('((88-86)*(4/2))')))
```
The idea is:
1. Tokenize the string to `22, +, 44`
2. Parse the tokens to: LHS, SYMBOL, RHS
3. Divide and conquer LHS, RHS recusively
4. Perform math calculation when see `token_a SYM token_b` (no Parentheses)


## Review
[Tracking the Money — Scaling Financial Reporting at Airbnb](https://medium.com/airbnb-engineering/tracking-the-money-scaling-financial-reporting-at-airbnb-6d742b80f040)
- Airbnb supports 190+ countries, with 70+ currencies and 20+ processors.
- Airbnb has to track money in, and money out. 
- Lots of transactions on every single day.
- They re-architect their system from a MySQL-based data pipeline to event based pipeline.
- Use Apache Spark, written in Scala. They chose Scala as the language because we wanted the latest features of Spark, as well as the other benefits of the language, like types, closures, immutability, lazy evaluation, etc.


{{% admonition example "Key Takeaways" %}}
- Use [double entry accounting](https://en.wikipedia.org/wiki/Double-entry_bookkeeping). Double entry accounting allows us to be sure that everything is accounted for properly in the system. This means no money appears or disappears without a source.
- Nightly processing is too slow, comparing to event based system.
- decouple the financial logic from the product logic.
 {{% /admonition %}}

## Share
{{% admonition tip "share" %}}
How to study?
{{% /admonition %}}

#### How to understand a topic?
- Understand big picture, read source code. 
- You must write code to be fully understand it in and out.
- Knowing a concept and reading some articles are not enough. You need to have an implementation level of understanding. It will then live inside your brain forever and become part of your knowledge.

#### How to read source code?
- You don’t read source code arbitrary, you come up with questions and wonder how it works in certain way. 
- You search the answers in source code.
- Once you found out the solution to your questions, you understand the system, and how it works deeply.
- You summarize what you found in blog.
- You explain this cool thing to your friends, your wife/husband. You might find some missing pieces while answering questions from them.
- Lastly, you wrote your implementation to demonstrate this idea.

## Tips
[autoenv](https://github.com/inishchith/autoenv)
If a directory contains a .env file, it will automatically be executed when you cd into it. 

When enabled (set AUTOENV_ENABLE_LEAVE to a non-null string), if a directory contains a .env.leave file, it will automatically be executed when you leave it.
```
$ echo "echo 'whoa'" > project/.env
$ cd project
whoa
```