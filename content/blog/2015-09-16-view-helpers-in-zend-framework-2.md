---
title: View Helpers in Zend Framework 2
author: grrrben
type: post
date: 2015-09-16T17:01:05+00:00
url: /blog/view-helpers-in-zend-framework-2/
categories:
  - php
tags:
  - php
  - zend
  - zf2

---
View helpers in Zend Framework 2 werken goed, maar de werkwijze is net even anders dan bij versie 1. Zoals het hele Framework natuurlijk, je moet het even door hebben, maar ZF2 biedt je uiteindelijk betere code in minder regels.<!--more-->

Het basisgebruik, een view helper die met meerdere methods is een class die `Zend\View\Helper\AbstractHelper` extend en enkele gewoon de benodigde methods bevat om je view gerelateerde logica uit te voeren.

Voorbeeld file: `App/src/App/View/Helper/Strings.php`

    <?php
    /**
     * @author gerben
     * @url http://www.atog.nl/blog/view-helpers-in-zend-framework-2/
     */
    
    namespace Daghap\View\Helper;
    
    use Zend\View\Helper\AbstractHelper;
    
    class Strings extends AbstractHelper {
    
     /**
     * @var some\class
     */
     private $_ciClass;
    
     public function __construct($codeInjectedClass) {
     // soms
    $this->_ciClass = $codeInjectedClass;
     }
    
     /**
     * @param string $foo
     * @return string
     */
     public function foo ($foo) {
     return strtoupper($foo);
     }
     
     /**
     * @param string $bar
     * @return string
     */
     public function bar ($bar) {
     // using a view helper in a view helper
     $translation = $this->view->translate("bar");
     return strtolower($translation);
     }
    
     
    }

View helpers kunnen view helpers gebruiken. De extended abstractHelper class `Zend\View\Helper\AbstractHelper` maakt het view object als variabele $this->view beschikbaar. Helpers aanroepen gaat dus via `$this->view->helperName();`.

    $translation = $this->view->translate("bar");

View helpers kunnen ook direct hun return value teruggeven. Hiervoor wordt in ZF2 de `__invoke()` method gebruikt.

Voorbeeld file: `App/src/App/View/Helper/Datum.php`

    <?php
    
    class Datum extends AbstractHelper {
    
     // knip ...
    
     public function __invoke($mysqlDateFormat) {
     
     if ($mysqlDateFormat == date("Y-m-d")) {
     $dateString = 'vandaag';
     } elseif ($mysqlDateFormat == date("Y-m-d", strtotime("+1 days"))) {
     $dateString = 'morgen';
     } elseif ($mysqlDateFormat == date("Y-m-d", strtotime("+2 days"))) {
     $dateString = 'overmorgen';
     } else {
     $dateString = date("j", $time) . ' ' . $monthNL;
     }
    
     return $dateString;
    
     }
    }

Een aanroep via `$this->datum($date)` in een view geeft direct de string (bijv) &#8216;vandaag&#8217; terug

Bij de module config file maak je de view helpers beschikbaar.
  
Voorbeeld file: `App/config/application.config.php`

    
    'view_helpers' => array(
    'invokables' => array(
    'datum' => 'Daghap\View\Helper\Datum',
    'strings' => 'Daghap\View\Helper\Strings',
    ),
    ),

Aanroepen vanuit de view gaat als volgt:

<pre><code class="php">echo $this-&gt;strings()-&gt;repeatDaysString($int);
echo $this-&gt;datum($date);</code></pre>