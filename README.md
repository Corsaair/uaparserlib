uaparserlib
===========

A lightweight User-Agent string parser.  
Library ported to ActionScript 3.0 from [faisalman/ua-parser-js](https://github.com/faisalman/ua-parser-js).

Install
-------

This AS3 library is meant to be used in the context of [Redtamarin](https://github.com/Corsaair/redtamarin)
or other utilities like [as3shebang](https://github.com/Corsaair/as3shebang).

We do not have yet a package manager for Redtamarin, so you will have
to install "by hand" (eg. copy the right file at the right place).


Usage
-----

**ABC library**

```as3
var uaparserlib:* = Domain.currentDomain.load( "uaparserlib.abc" );
trace( uaparserlib + " loaded"  ); //optional

import libraries.uaparser.*;

//use any public definitions of the uaparser library
```

**sources**

Copy `uaparserlib/src` to your current AS3 project path.

In your main AS3 file
```as3
include "uaparserlib.as";

import libraries.uaparser.*;

//use any public definitions of the uaparser library
```

Example
-------

```as3
import libraries.uaparser.UAParser;

var ua:String = "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/44.0.2403.157 Safari/537.36";

var obj:* = UAParser( ua );

if( obj.browser.name == "Chrome" )
{
	trace( "found Google Chrome" );
}

```

Porting Notes
-------------

`ua-parser-js` use mainly vanilla js without dependencies,
so it's a perfect example to show how easy it can be to port a JS library to an AS3 library.



Comment out  
`(function (window, undefined) {`  
and  
`})(typeof window === 'object' ? window : this);`

and replace them by  
`package libraries.uaparser {`  
and  
`}`

comment out  
`'use strict';`

By default, all package definitions are declared **internal**
so you need to define at least one **public** definition.

in this case  
`var UAParser = function (uastring, extensions) {`  
become  
`public var UAParser = function (uastring, extensions) {`

Edit Browser DOM objects  
`var ua = uastring || ((window && window.navigator && window.navigator.userAgent) ? window.navigator.userAgent : EMPTY);`  
become  
`var ua = uastring;`

Finally, comment out JS export functions/utilities  
```js
    ///////////
    // Export
    //////////


    // check js environment
    if (typeof(exports) !== UNDEF_TYPE) {
        // nodejs env
        if (typeof module !== UNDEF_TYPE && module.exports) {
            exports = module.exports = UAParser;
        }
        exports.UAParser = UAParser;
    } else {
        // requirejs env (optional)
        if (typeof(define) === FUNC_TYPE && define.amd) {
            define(function () {
                return UAParser;
            });
        } else {
            // browser env
            window.UAParser = UAParser;
        }
    }

    // jQuery/Zepto specific (optional)
    // Note: 
    //   In AMD env the global scope should be kept clean, but jQuery is an exception.
    //   jQuery always exports to global scope, unless jQuery.noConflict(true) is used,
    //   and we should catch that.
    var $ = window.jQuery || window.Zepto;
    if (typeof $ !== UNDEF_TYPE) {
        var parser = new UAParser();
        $.ua = parser.getResult();
        $.ua.get = function() {
            return parser.getUA();
        };
        $.ua.set = function (uastring) {
            parser.setUA(uastring);
            var result = parser.getResult();
            for (var prop in result) {
                $.ua[prop] = result[prop];
            }
        };
    }
```