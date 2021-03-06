Value formatters in DOCX and XLSX templates
===========================================

You can use formatters to add complex logic to values rendered in your templates. For example, you can change value format, hide content, join arrays, etc.

This article covers formatters for both DOCX and XLSX templates.

.. contents:: List of formatters
   :local:
   :depth: 1

format
------

It formats a tag value. You can use it with or without parameters:

- :code:`format` - if encountered on date value it will format it as short date string.
- :code:`format(val)` - formats current value using specified format string. For example, you can use :code:`N2` for a numbers with two decimals.

It uses standard format strings. You can find more information in Microsoft documentation:

- `Numeric format strings <https://docs.microsoft.com/en-us/dotnet/standard/base-types/standard-numeric-format-strings>`_
- `Date and time format strings <https://docs.microsoft.com/en-us/dotnet/standard/base-types/standard-date-and-time-format-strings>`_

Examples
~~~~~~~~

.. list-table::
    :header-rows: 1

    *   - Template
        - Data
        - Result
    *   - .. code-block:: json
    
            Date: {{date}:format(d MMM yyyy)}
            Date: {{date}:format(MM/dd)}
            Date: {{date}:format(U)}
            Number: {{num}:format(C)}
            Number: {{num}:format(P)}
            Number: {{num}:format(N2)}

        - .. code-block:: json

            {                     
                "date": "2012-04-21T18:25:43-05:00",
                "num": 8
            }        

        - .. code-block:: json
    
            Date: 22 Apr 2012
            Date: 04/22
            Saturday, April 21, 2012 11:25:43 PM
            Number: $8.00
            Number: 800.00%
            Number: 8.00
   
substring
---------

Returns substring. You can use the formatter with one or two parameters:

- :code:`substring(index)` - returns substring of provided values after :code:`index` chars
- :code:`substring(index,length)` - returns substring of provided values after :code:`index` with :code:`length`.

Examples
~~~~~~~~

.. list-table::
    :header-rows: 1

    *   - Template
        - Data
        - Result
    *   - .. code-block:: json
    
            {{stringVal}:substring(6)}
            {{stringVal}:substring(0, 5)}     

        - .. code-block:: json

            {                     
                "stringVal" "Derek Clark"
            }        

        - .. code-block:: json
    
            Clark
            Derek        

join
----

:code:`join(separator)` - joins array values with a :code:`separator`.

Examples
~~~~~~~~

.. list-table::
    :header-rows: 1

    *   - Template
        - Data
        - Result
    *   - .. code-block:: json
    
            {{arr}:join(, )}
            {{arr}:join(; )
            {{arr}:join(-)}

        - .. code-block:: json

            {                     
                "arr" [ 1, 2, 3]
            }        

        - .. code-block:: json
    
            1, 2, 3
            1; 2; 3
            1-2-3      

offset
------

:code:`offset(d)` - date and time value will be offset by :code:`d` days. 

:code:`offset(d.hh\:mm\:ss)` - advanced approach for offsetting :code:`d` days, :code:`hh` hours, :code:`mm` minutes, :code:`ss` seconds. 

Just replace :code:`d`, :code:`hh`, :code:`mm` and :code:`ss` by the required number of days, hours, minutes and seconds in this string pattern :code:`d.hh\:mm\:ss`.

Examples
~~~~~~~~

.. list-table::
    :header-rows: 1

    *   - Template
        - Data
        - Result
    *   - .. code-block:: json
    
            Without offset:
            {{date}}
            
            Plus 10 days
            {{date}:offset(10)}
            
            Minus 10 days:
            {{date}:offset(-10)}
            
            Plus 10 days, 1 hour, 
            5 minutes, 10 seconds:
            {{date}:offset(10.1\:5\:10)}
            
            Minus 10 days, 1 hour, 
            5 minutes, 10 seconds:
            {{date}:offset(-10.1\:5\:10)}

        - .. code-block:: json

            {                     
                "date" "2012-04-21T18:25:43-05:00"
            }        

        - .. code-block:: json
    
            Without offset:
            4/22/2012 3:25:43 AM
            
            Plus 10 days
            5/2/2012 3:25:43 AM
            
            Minus 10 days:
            4/12/2012 3:25:43 AM
            
            Plus 10 days, 1 hour, 
            5 minutes, 10 seconds:
            5/2/2012 4:30:53 AM

            Minus 10 days, 1 hour, 
            5 minutes, 10 seconds:
            4/12/2012 2:20:33 AM                                        

hide
----

:code:`hide` - replaces current tag value with an empty string. It can be used to hide the content of a specific tag.

Examples
~~~~~~~~

.. list-table::
    :header-rows: 1

    *   - Template
        - Data
        - Result
    *   - .. code-block:: json
    
            {{val1}}
            {{val2}:hide}

        - .. code-block:: json

            {                     
                "val1" "Derek Clark",
                "val2" "Jessica Adams"
            }         

        - .. code-block:: json
    
            Derek Clark

collapse
--------

:code:`collapse` - it can be used to conditionally hide blocks of a Word document or to clear cells in an Excel document. If a value in the tag is :code:`null`, :code:`true`, empty or empty array, it will be applied.

Examples
~~~~~~~~

The formatter can be used in both DOCX and XLSX templates. However, it behaves differently for them. Read the articles below for more information:

- `How to hide content blocks in DOCX templates <../docx/conditionally-hide-blocks.html>`_
- `How to clear cells in XLSX templates <../xlsx/conditionally-clear-cells.html>`_

bool
----

:code:`bool(yes,no,maybe)` - boolean value will be converted to :code:`yes`, :code:`no` or :code:`maybe`. You can specify your own value for each state. The last parameter is optional. You can use it if your bool value can be null.

Examples
~~~~~~~~

.. list-table::
    :header-rows: 1

    *   - Template
        - Data
        - Result
    *   - .. code-block:: json
    
            {{boolVal1}:bool(yes,no,maybe)}
            {{boolVal2}:bool(yes,no,maybe)}
            {{boolVal3}:bool(yes,no,maybe)}

        - .. code-block:: json

            {                     
                "boolVal1": true,
                "boolVal2": false,
                "boolVal3": null,
            }         

        - .. code-block:: json
    
            yes
            no
            maybe

empty
-----

:code:`empty(val)` - if a value in a tag is null, empty or empty array it will replace the value with :code:`val`. You can use this formatter to display default value. For example, "N/A".

Examples
~~~~~~~~

.. list-table::
    :header-rows: 1

    *   - Template
        - Data
        - Result
    *   - .. code-block:: json
    
            {{val1}:empty(N/A)}
            {{val2}:empty(N/A)}
            {{val3}:empty(N/A)}


        - .. code-block:: json

            {                     
                "val1": "Jessica Adams",
                "val2": "",
                "val3": [],
            }         

        - .. code-block:: json
    
            Jessica Adams
            N/A
            N/A

merge-nulls
-----------

:code:`merge-nulls` - use this formatter to merge table cells horizontally if there is null value.

.. note:: This formatter can be used in both DOCX and XLSX templates. However, Excel doesn't support merging cells in table ranges. Thus, if you want to use this formatter, apply it to regular Excel cells instead. See the example below.

Examples
~~~~~~~~

.. list-table::
    :header-rows: 1

    *   - Template
        - Data
        - Result
    *   - 
    
            DOCX template:
        
            .. image:: ../../_static/img/document-generation/merge-nulls-template.png
                :alt: merge nulls formatter template

            XLSX template (`download <../../_static/files/document-generation/demos/merge-nulls-template.xlsx>`_):

            .. image:: ../../_static/img/document-generation/xlsx-merge-nulls-template.png
                :alt: merge nulls formatter template

        - .. code-block:: json

            {         
                "collection": [
                    {
                        "name": "Derek Clark",
                        "sold": null
                    },
                    {
                        "name": "Jessica Adams",
                        "sold": 14000
                    },
                    {
                        "name": "Xue Li",
                        "sold": null
                    },
                    {
                        "name": "Martin Huston",
                        "sold": 9400
                    },
                    {
                        "name": "Anton Frolov",
                        "sold": null
                    }
                ]
            }        

        - 
        
            Cells with null values were merged.

            DOCX result:
        
            .. image:: ../../_static/img/document-generation/merge-nulls-result.png
                :alt: merge nulls fortammer result

            XLSX result (`download <../../_static/files/document-generation/demos/merge-nulls-result.xlsx>`_):
        
            .. image:: ../../_static/img/document-generation/xlsx-merge-nulls-result.png
                :alt: merge nulls fortammer result

span-nulls
-----------

:code:`span-nulls` - use this formatter to merge table cells vertically if there is null value.

.. important:: This formatter can be used in DOCX templates only.

Examples
~~~~~~~~

.. list-table::
    :header-rows: 1

    *   - Template
        - Data
        - Result
    *   - 

            `Download template document <../../_static/files/document-generation/demos/span-nulls-template.docx>`_                
        
            .. image:: ../../_static/img/document-generation/span-nulls-template.png
                :alt: span nulls formatter template


        - .. code-block:: json

            {
                "collection": [
                    {
                        "name": "Derek Clark",
                        "sold": null,
                        "period": "Jan 2018"
                    },
                    {
                        "name": "Jessica Adams",
                        "sold": 14000,
                        "period": "Feb 2018"
                    },
                    {
                        "name": "Xue Li",
                        "sold": null,
                        "period": "Mar 2018"
                    },
                    {
                        "name": "Martin Huston",
                        "sold": 9400,
                        "period": "May 2018"
                    },
                    {
                        "name": "Anton Frolov",
                        "sold": null,
                        "period": "Jun 2018"
                    }        
                ]
            }       

        - 
        
            `Download result document <../../_static/files/document-generation/demos/span-nulls-result.docx>`_
        
            .. image:: ../../_static/img/document-generation/span-nulls-result.png
                :alt: span nulls fortammer result            

horizontal-resize
-----------------

:code:`horizontal-resize` - it can be used to repeat collections horizontally instead of vertically in Excel. See the example below.

.. important:: This formatter can be used in XLSX templates only.

Examples
~~~~~~~~

.. list-table::
    :header-rows: 1

    *   - Template
        - Data
        - Result
    *   - .. image:: ../../_static/img/document-generation/horizontal-resize-template.png
            :alt: horizontal-resize formatter template

        - .. code-block:: json

            {                     
                "collection": [
                    {
                        "name": "Derek Clark"                        
                    },
                    {
                        "name": "Jessica Adams"                        
                    },
                    {
                        "name": "Xue Li"
                    }
                ]
            }         

        - 
        
            New columns are added instead of new rows:
        
            .. image:: ../../_static/img/document-generation/horizontal-resize-result.png
                :alt: horizontal-resize formatter result


page
----

:code:`page` - it can be used for changing the logic of repeating collections. When a tag is placed inside the table and you want to repeat entire page instead of a table row, use :code:`page` to override default repeating logic.

.. important:: This formatter can be used in DOCX templates only.

Examples
~~~~~~~~

.. list-table::
    :header-rows: 1

    *   - Template
        - Data
        - Result
    *   - .. image:: ../../_static/img/document-generation/page-formatter-template.png
            :alt: page formatter template

        - .. code-block:: json

            {                     
                "collection": [
                    {
                        "name": "Derek Clark",
                        "sold": 10000
                    },
                    {
                        "name": "Jessica Adams",
                        "sold": 14000
                    },
                    {
                        "name": "Xue Li",
                        "sold": 9400
                    }
                ]
            }         

        - 
        
            New pages are added instead of new table rows:
        
            .. image:: ../../_static/img/document-generation/page-formatter-result.png
                :alt: page formatter result

sheet
-----

:code:`sheet` - it can be used for changing the logic of repeating collections. When a tag is placed inside the table and you want to create a separate sheet for each collection item instead of a table row, use :code:`sheet` to override default repeating logic.

.. important:: This formatter can be used in XLSX templates only.

Examples
~~~~~~~~

.. list-table::
    :header-rows: 1

    *   - Template
        - Data
        - Result
    *   - .. image:: ../../_static/img/document-generation/sheet-formatter-template.png
            :alt: sheet formatter template

        - .. code-block:: json

            {                     
                "collection": [
                    {
                        "name": "Derek Clark",
                        "sold": 10000
                    },
                    {
                        "name": "Jessica Adams",
                        "sold": 14000
                    },
                    {
                        "name": "Xue Li",
                        "sold": 9400
                    }
                ]
            }         

        - 
        
            New sheets are added instead of new table rows:
        
            .. image:: ../../_static/img/document-generation/sheet-formatter-result.png
                :alt: sheet formatter result