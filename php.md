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



