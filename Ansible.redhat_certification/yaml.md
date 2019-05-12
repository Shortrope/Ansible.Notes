# Yaml

## Example

    ---
    # An employee record
    name: Martin D'vloper
    job: Developer
    skill: Elite
    employed: True
    foods:
        - Apple
        - Orange
        - Strawberry
        - Mango
    languages:
        perl: Elite
        python: Elite
        pascal: Lame
    education: |
        4 GCSEs
        3 A-Levels
        BSc in the Internet of Things
    Composed of 
    - key-value pairs
    - lists
    - dictionaries
    ...

Files open w three hyphens '---'  
Ans close w three periods '...'


## Comment
    ---# Grocery List

## List
    ---# Grocery List
    - Bananas
    - Apples
    - Oranges
    - Derial
    - Eggs

or

    [Bananas, Apples, Oranges, Derial, Eggs]

## Associative Arrays or Key:Value pairs
Strings do not require quotes

    ---# Employee Info (List)
    Name: John Smith
    Age: 44
    HireDate: 09/01/2019

    ---# Employee Info (Inline)
    {Name: John Smith, Age: 44, HireDate: 09/01/2019}

## Dictionaries
**colon** and a **space**  
followed by **indented** key-value pairs

    tasks:
    - name: something
      yum: name=httpd
    -name: something 2
      apt: mysql

## New Line Preservation

### greater than '>' ignores line breaks
This allows you to break a line into multiple lines for readabiltiy

    template: src=/srv/httpd.j2 dest=/etc/httpd.conf
vs

    template: >
      src=/srv/httpd.j2 
      dest=/etc/httpd.conf

### Pipe 
Takes remembers line breaks as part of the input

    ---# Format as is
    include_newlines: |
        Now is the time for all good men to
        Come to the aid of their country. Now for a
        famous quote to display
            Four score and seven years ago

    --# New line Folding
    fold_newlines: >
        This is really a
        single line of text
        despite appearances

    Blank lines signify a new paragraph

## Quotes and Escaping special chars
- Special chars: [ ] { } : > |  
- Must use **double quotes** to escape special chars  
- Only need to escape a : if it is followed by a space. But generally quote the whole line  
- Curly braces are used for varialbles "{{var}}"  
- Booleans are auto-converted in yaml.  So dquote if want literal "Yes" "True"  
- Floats are taken as numeric unless quoted


## Data Types

    ---
    a: 123              # integer
    b: "123"            # string
    c: 123.0            # float
    d: !!float 123      # float
    e: !!str 123        # string
    f: Yes              # boolean True (yaml 1.1), string (yaml 1.2)
    g: Yes we have no bananas # string


