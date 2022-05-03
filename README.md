# docassemble.socialsec

A set of questions used to determine where social security is due in Europe

## Installation

This repository can readily be imported into docassemble, using the guide available [here](https://docassemble.org/docs/packages.html#installing).
The actual installation of docassemble can take many forms, thankfully the documentation has [a well-documented setup available through docker](https://docassemble.org/docs/docker.html#https).
It even has [built-in support for HTTPS](https://docassemble.org/docs/docker.html#https).

## Development

All the code for the 'interview' is available in `docassemble/socialsec/data/questions`.
The `interview.yml` file simply imports the necessary code/language files, and will only need to be edited to include new ones.

### Languages

The `en.yml` file provides a good boilerplate for the values that need to be provided to the interview.
In general, any of the strings in that file need to be localised, whereas variable names (that will also be written in English) need to stay English as they provide the backbone of the actual interview.

For example `  loc_outcome = 'Outcome'` requires `'Outcome'` to be changed to a different language, while `loc_outcome` needs to stay untouched.
Similarly, in this example:
```
  civil_servant_yn.question = \
    'Are you a civil servant?'
  civil_servant_yn.subquestion = \
    'Please note: ensure that you are treated as a civil servant under current legislation. This has recently changed for people in Dutch education under the "Wet Normalisering Rechtspositie Ambtenaren", who are no longer treated as civil servant.'
```
`civil_servant_yn.question` and `civil_servant_yn.subquestion` are logic and need to stay English, the quoted strings containing the actual question can be changed to a different language. A Dutch version of the interview is available under 'nl.yml'.

In the case of 'native' docassemble questions (notices etc.), change the `question` and `subquestion`, but don't touch the `continue button field`.

### Flowchart structure

`code.yml` contains the structure and logic of the flowchart, with the large `code` block defining how the questions should be asked in order.
If new questions need to be added, they need to be initialized in the `objects` block and `all_questions` variable. 
They can then be added into the localized files (`en.yml` etc) following the structure of existing questions.
The method of branching should be obvious enough from the main `code` block.

### Question logic

The way questions are structured is a bit complex, mostly due to the need for localization and review functionality.
Questions are initialized in the `objects` block at the top of `code.yml`, to be able to assign question bodies to them.
They then need to be included in the `all_questions` list in order to be picked up by the review at the end of the flowchart.

When a question is encountered in the interview logic, docassemble will look for `<question>.answer`.
The only place where it can find this specific variable is in the "catch all" `generic_object` question defined in `code.yml`.
This question is essentially a template that forces a lookup for `<question>.question` (and possibly `<question>.subquestion`).
These need to be defined in the `<language>.yml` files.
Notices etc. instead follow a more traditional docassemble method for definitions, as they do not need to be templated in the same way.

## Author

Maastricht Law & Tech Lab, Maastricht University / Brightlands Institute for Smart Society (BISS)
