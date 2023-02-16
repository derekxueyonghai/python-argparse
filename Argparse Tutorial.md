[Argparse Tutorial](https://docs.python.org/3/howto/argparse.html)

- positional argument\
- optional argument\

prog.py\


	import argparse
	parser = argparse.ArgumentParser()
	parser.add_argument("echo")
	args = parser.parse_args()
	print(args.echo)
	
Add help description

	import argparse
	parser = argparse.ArgumentParser()
	parser.add_argument("echo", help="echo the string you use here")
	args = parser.parse_args()
	print(args.echo)
	
how about doing something even more useful:

	import argparse
	parser = argparse.ArgumentParser()
	parser.add_argument("square", help="display a square of a given number")
	args = parser.parse_args()
	print(args.square**2)

Run:
	
	python3 prog.py 4

Let’s tell argparse to treat that input as an integer:

	import argparse
	parser = argparse.ArgumentParser()
	parser.add_argument("square", help="display a square of a given number", type=int)
	args = parser.parse_args()
	print(args.square**2)

Run:
	
	python3 prog.py 4
	
	
Introducing Optional arguments

	import argparse
	parser = argparse.ArgumentParser()
	parser.add_argument("--verbosity", help="increase output verbosity")
	args = parser.parse_args()
	if args.verbosity:
	    print("verbosity turned on")
	    
Let’s modify the code accordingly:

	import argparse
	parser = argparse.ArgumentParser()
	parser.add_argument("--verbose", help="increase output verbosity",
	                    action="store_true")
	args = parser.parse_args()
	if args.verbose:
	    print("verbosity turned on")
	    
Combining Positional and Optional arguments

	import argparse
	parser = argparse.ArgumentParser()
	parser.add_argument("square", type=int,
	                    help="display a square of a given number")
	parser.add_argument("-v", "--verbose", action="store_true",
	                    help="increase output verbosity")
	args = parser.parse_args()
	answer = args.square**2
	if args.verbose:
	    print(f"the square of {args.square} equals {answer}")
	else:
	    print(answer)
	    
Give this program of ours back the ability to have multiple verbosity values, and actually get to use them:

	import argparse
	parser = argparse.ArgumentParser()
	parser.add_argument("square", type=int,
	                    help="display a square of a given number")
	parser.add_argument("-v", "--verbosity", type=int,
	                    help="increase output verbosity")
	args = parser.parse_args()
	answer = args.square**2
	if args.verbosity == 2:
	    print(f"the square of {args.square} equals {answer}")
	elif args.verbosity == 1:
	    print(f"{args.square}^2 == {answer}")
	else:
	    print(answer)
	    
Restricting the values the --verbosity option can accept:

	import argparse
	parser = argparse.ArgumentParser()
	parser.add_argument("square", type=int,
	                    help="display a square of a given number")
	parser.add_argument("-v", "--verbosity", type=int, choices=[0, 1, 2],
	                    help="increase output verbosity")
	args = parser.parse_args()
	answer = args.square**2
	if args.verbosity == 2:
	    print(f"the square of {args.square} equals {answer}")
	elif args.verbosity == 1:
	    print(f"{args.square}^2 == {answer}")
	else:
	    print(answer)
	    

Introduced another action, “count”, to count the number of occurrences of specific options.

	import argparse
	parser = argparse.ArgumentParser()
	parser.add_argument("square", type=int,
	                    help="display the square of a given number")
	parser.add_argument("-v", "--verbosity", action="count",
	                    help="increase output verbosity")
	args = parser.parse_args()
	answer = args.square**2
	if args.verbosity == 2:
	    print(f"the square of {args.square} equals {answer}")
	elif args.verbosity == 1:
	    print(f"{args.square}^2 == {answer}")
	else:
	    print(answer)
	    
Getting a little more advanced.

	import argparse
	parser = argparse.ArgumentParser()
	parser.add_argument("x", type=int, help="the base")
	parser.add_argument("y", type=int, help="the exponent")
	parser.add_argument("-v", "--verbosity", action="count", default=0)
	args = parser.parse_args()
	answer = args.x ** args.y
	if args.verbosity >= 2:
	    print(f"{args.x} to the power {args.y} equals {answer}")
	elif args.verbosity >= 1:
	    print(f"{args.x}^{args.y} == {answer}")
	else:
	    print(answer)
	    
The following example instead uses verbosity level to display more text instead:

	import argparse
	parser = argparse.ArgumentParser()
	parser.add_argument("x", type=int, help="the base")
	parser.add_argument("y", type=int, help="the exponent")
	parser.add_argument("-v", "--verbosity", action="count", default=0)
	args = parser.parse_args()
	answer = args.x**args.y
	if args.verbosity >= 2:
	    print(f"Running '{__file__}'")
	if args.verbosity >= 1:
	    print(f"{args.x}^{args.y} == ", end="")
	print(answer)
	
Conflicting options

	import argparse
	
	parser = argparse.ArgumentParser()
	group = parser.add_mutually_exclusive_group()
	group.add_argument("-v", "--verbose", action="store_true")
	group.add_argument("-q", "--quiet", action="store_true")
	
	parser.add_argument("x", type=int, help="the base")
	parser.add_argument("y", type=int, help="the exponent")
	
	args = parser.parse_args()
	answer = args.x ** args.y
	
	if args.quiet:
	    print(answer)
	elif args.verbose:
	    print(f"{args.x} to the power {args.y} equals {answer}")
	else:
	    print(f"{args.x}^{args.y} == {answer}")
	    
Tell your users the main purpose of your program, just in case they don’t know:

	import argparse
	
	parser = argparse.ArgumentParser(description="calculate X to the power of Y")
	group = parser.add_mutually_exclusive_group()
	group.add_argument("-v", "--verbose", action="store_true")
	group.add_argument("-q", "--quiet", action="store_true")
	parser.add_argument("x", type=int, help="the base")
	parser.add_argument("y", type=int, help="the exponent")
	args = parser.parse_args()
	answer = args.x ** args.y
	
	if args.quiet:
	    print(answer)
	elif args.verbose:
	    print(f"{args.x} to the power {args.y} equals {answer}")
	else:
	    print(f"{args.x}^{args.y} == {answer}")