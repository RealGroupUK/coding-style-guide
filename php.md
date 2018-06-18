# PHP Style Guide

A good amount of this is "borrowed" from the WordPress style guide as a fair amount of 
Real Group development is working on WordPress code and plugins.

There are differences though, and also additional items not documented there.

## Tabs and spaces

Any code indentation must be indented to four spaces. Configure your IDE to replace **tabs** with **spaces**
to ensure that indentation remains the same in any editor.

### Associative arrays

Each item in an associative array should start on a new, indented line when the array contains more than one item:

    $array = (
        'foo' => 'bar',
        'hello' => 'world',
    );

## Single and double quotes

Use single and double quotes when appropriate. If you’re not evaluating anything in the string,
use single quotes. You should almost never have to escape quotes in a string, because you can
just alternate your quoting style, like so:

    echo '<a href="/static/link" title="Yeah yeah!">Link name</a>';
    echo "<a href='$link' title='$linktitle'>$linkname</a>";
    
## Arrays

### Final commas

Final items in arrays should include a comma. This makes it easer to change the order of the array
and provides cleaner diffs in version control.

    $my_array = array(
        'foo'   => 'somevalue',
        'foo2'  => 'somevalue2',
        'foo3'  => 'somevalue3',
        'foo34' => 'somevalue3',
    );

## Brace styles

Braces must be used for all code blocks in this style:

    if ( condition ) {
        action1();
        action2();
    } elseif ( condition2 && condition3 ) {
        action3();
        action4();
    } else {
        defaultaction();
    }

Braces should use the block style, even for single conditions or when they are not required. As such,
*single-statement inline control structures* must not be used.

## Use `elseif`, not `else if`

`if|else` conditionals should favour `elseif` for consistency with label blocks.

## Opening and closing PHP tags

When adding multi-line PHP snippets in HTML, PHP open and close tags should always be on a single 
line alone.

### Short hand PHP tags

Shorthand PHP tags must never be used.

### Remove trailing whitespace


## Naming conventions
### Class names
Class names should use capitalized words with no spaces or underscores. Any acronyms should be all uppe case.

    class MyNewClass extends MyClass { [...] }
    
### Method names
Methods within classes should use a `camelCasee` format. The first letter should be lowercase and subsequent words should start with and uppercase character.

    public function myFunction() { [...] }
    
### Constants
Constants should be in all upper-case with underscores separating words:
	
    define( 'MY_CONSTANT', true );

### File names
File names should match the name and case structure of the class within. Eg. `MyNewClass` should have the filename `MyNewClass.php`.

## Space usage
Always put spaces after commas, and on both sides of logical, comparison, string and assignment operators.

    x == 23
    foo && bar
    ! foo
    array( 1, 2, 3 )
    $baz . '-5'
    $term .= 'X'
    
Put spaces on both sides of the opening and closing parentheses of `if`, `elseif`, `foreach`, `for`, and `switch` blocks.
