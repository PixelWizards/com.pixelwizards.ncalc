## ncalc

NCalc - Mathematical Expressions Evaluator for .NET



NCalc is a mathematical expressions evaluator in .NET. NCalc can parse any expression and evaluate the result, including static or dynamic parameters and custom functions.

Originally from: https://archive.codeplex.com/?p=ncalc#

------

# IMPORTANT INFORMATION

This repository is no longer maintained. The source code has been moved to Github by James Cane and will provide support. https://github.com/sheetsync/NCalc

## Project Description

NCalc is a mathematical expressions evaluator in .NET. NCalc can parse any expression and evaluate the result, including static or dynamic parameters and custom functions.

For additional information on the technique we used to create this framework please read this article: http://www.codeproject.com/KB/recipes/sota_expression_evaluator.aspx.

For documentation here is the table of content:

- [description](https://ncalc.codeplex.com/wikipage?title=description&referringTitle=Home) : overall concepts, usage and extensibility points
- [operators](https://ncalc.codeplex.com/wikipage?title=operators&referringTitle=Home) : available standard operators and structures
- [values](https://ncalc.codeplex.com/wikipage?title=values&referringTitle=Home) : authorized values like types, functions, ...
- [functions](https://ncalc.codeplex.com/wikipage?title=functions&referringTitle=Home) : list of already implemented functions
- [parameters](https://ncalc.codeplex.com/wikipage?title=parameters&referringTitle=Home) : on how to use parameters expressions

## Functionnalities

**Simple expressions**



```
  Expression e = new Expression("2 + 3 * 5");
  Debug.Assert(17 == e.Evaluate());
```


**Evaluates .NET data types**



```
  Debug.Assert(123456 == new Expression("123456").Evaluate()); // integers
  Debug.Assert(new DateTime(2001, 01, 01) == new Expression("#01/01/2001#").Evaluate()); // date and times
  Debug.Assert(123.456 == new Expression("123.456").Evaluate()); // floating point numbers
  Debug.Assert(true == new Expression("true").Evaluate()); // booleans
  Debug.Assert("azerty" == new Expression("'azerty'").Evaluate()); // strings
```


**Handles mathematical functional from System.Math**



```
  Debug.Assert(0 == new Expression("Sin(0)").Evaluate());
  Debug.Assert(2 == new Expression("Sqrt(4)").Evaluate());
  Debug.Assert(0 == new Expression("Tan(0)").Evaluate());
```


**Evaluates custom functions**



```
  Expression e = new Expression("SecretOperation(3, 6)");
  e.EvaluateFunction += delegate(string name, FunctionArgs args)
      {
          if (name == "SecretOperation")
              args.Result = (int)args.Parameters[0].Evaluate() + (int)args.Parameters[1].Evaluate();
      };
  
  Debug.Assert(9 == e.Evaluate());
```


**Handles unicode characters**



```
  Debug.Assert("経済協力開発機構" == new Expression("'経済協力開発機構'").Evaluate());
  Debug.Assert("Hello" == new Expression(@"'\u0048\u0065\u006C\u006C\u006F'").Evaluate());
  Debug.Assert("だ" == new Expression(@"'\u3060'").Evaluate());
  Debug.Assert("\u0100" == new Expression(@"'\u0100'").Evaluate());
```


**Define parameters, even dynamic or expressions**



```
  Expression e = new Expression("Round(Pow([Pi], 2) + Pow([Pi2], 2) + [X], 2)");

  e.Parameters["Pi2"] = new Expression("Pi * [Pi]");
  e.Parameters["X"] = 10;

  e.EvaluateParameter += delegate(string name, ParameterArgs args)
    {
      if (name == "Pi")
      args.Result = 3.14;
    };

  Debug.Assert(117.07 == e.Evaluate());
```