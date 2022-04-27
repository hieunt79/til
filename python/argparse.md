# Add CLI to python script

The Python `argparse` lib:
- Allow the use of positional arguments
- Allow the customization of the prefix chars
- Support variable numbers of parameters for a single option
- Supports subcommands (A main command line parser can use other command line parsers depending on some arguments.)

## Tutorial

### Add helper, discription

First, import and create parser

```python
import argparse ...
# Create the parser
my_parser = argparse.ArgumentParser(prog='myls',
                                    description='List the content of a folder')

```
- `prog`: name of the program, will be used in help text
- `description`: content of description

### Add arguments

```python
my_parser.add_argument('Path',
                       metavar='path',
                       type=str,
                       help='the path to list')
```

### Execute parse_args()

```python
# Execute parse_args()
args = my_parser.parse_args()

input_path = args.Path
```

### Process arguments

E.g:

```python
if not os.path.isdir(input_path):
    print('The path specified does not exist')
    sys.exit()

print('\n'.join(os.listdir(input_path)))
```


## Ref:

- https://realpython.com/command-line-interfaces-python-argparse/