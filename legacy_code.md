# Legacy code

## Legacy code signs 

- it's built with old technologies: old versions of the programming language, libraries, packages;
- it's hard to understand and difficult to change;
- it doesn't have tests that secure developers from bugs;

## How to avoid legacy code

### Update version of programming language

A client has a site with legacy PHP 7.x

Reasons:

- Site uses legacy PHP framework Yii 1.
- Site is a monolith that do many things, no full documentation, no acceptance tests.
- It's scary to do any PHP version updates.
- No docker. With docker it's possible to update PHP version easily.

### Update version of your Framework

A client has:

- site with legacy Yii 1 Framework;
- site on Drupal 7.

Reasons:

- Code was written in Monolythic style, without modules.
- It's hard to say what things this app does.
- Most of code doesn't have unit, acceptance tests.
- Developers didn't install Drupal via composer, so it's hard to update it.
