# ICU Translation Bundle #

[![Build Status](https://travis-ci.org/webfactory/icu-translation-bundle.png?branch=master)](https://travis-ci.org/webfactory/icu-translation-bundle)
[![Coverage Status](https://coveralls.io/repos/webfactory/icu-translation-bundle/badge.png?branch=master)](https://coveralls.io/r/webfactory/icu-translation-bundle?branch=master)

While the [Symfony2 translation component](http://symfony.com/doc/current/components/translation/index.html) does a
great job in most cases, it can become difficult to use if you need conditions other than numbers (e.g. gender) or
nested conditions. This is where the ICU Translation Bundle steps in. Using the [International Components for Unicode
project](http://site.icu-project.org/)'s standard message format, it enhances the Symfony component with arbitrary and
nested conditions, as well as easy-to-use localized number and date formatting. The enhancement is non-invasive, i.e.
you don't have to touch your former messages, they'll still work as usual.

## Installation ##

Assuming you've already [enabled and configured the Symfony2 translation component](http://symfony.com/doc/current/book/translation.html#book-translation-configuration),
all you have to do is to install the bundle via [composer](https://getcomposer.org) with something like this:

    php composer.phar require webfactory/icu-translation-bundle

(We use [Semantic Versioning](http://semver.org/), so as soon as a version tagged 1.0.0 is available, you'll probably
want to use something like ~1.0 as the version string.)

As usual, enable the bundle in your kernel:

    <?php
    // app/AppKernel.php

    public function registerBundles()
    {
        $bundles = array(
            // ...
            new \Webfactory\IcuTranslationBundle\WebfactoryIcuTranslationBundle()
        );
        // ...
    }

## Usage ##

To use the bundle's enhancements, you need to use a special syntax with curly braces in your translation messages. The
following examples show this syntax for common use cases. For a comprehensive list, please refer to [in-depth
documentation](Resources/doc/index.rst).

### Number formatting with a parameter type ###

In your messages, you can specify "number" as a parameter type after a variable. If so, the output is localized with the
correct thousands separator and decimal mark. See this example message stored under a message key "message-key":

    1 mile = {mile_to_metres, number} metres

In a controller, the example could look like this:

    $output = $this->get('translator')->trans(
        'message-key',
        array('%mile_to_metres%' => 1609.34)
    );

E.g. for the locale "en", the output will be "1 mile = 1,609.34 metres", while for the locale "de" it will be "1 mile =
1.609,34 metres" (or "1 Meile = 1.609,34 Meter" with a proper translation).

For other parameter types such as date, see the in-depth documentation.

### Gender Specific Translations ###

Conditions can be used to realize gender specific translations.
The following message expects the parameter "gender_of_participant", which is
either "female" or "male":

    {gender_of_participant, select,
        female {She participated in the course.}
        other {He participated in the course.}
    }

Depending on the parameter value, the correct translation will be chosen.

### Pluralization ###

This translation message expects an integer value as "number_of_participants" parameter:

    {number_of_participants, plural,
        =0 {Nobody is participating.}
        =1 {One person participates.}
        other {# persons are participating.}
    }

Depending on the actual number the correct translation is selected. The *#* in
the last translation will be substituted by the value of "number_of_participants".

Please refer to [advanced documentation](Resources/doc/index.rst) for more details about the formatting options.