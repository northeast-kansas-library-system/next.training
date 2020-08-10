################
H1: Titles/Parts
################

(double space before chapter)

***********
H2: Chapter
***********

(double space before section)

H3: Section
===========

H4: Sub-section
---------------

H5: Sub-sub-section
^^^^^^^^^^^^^^^^^^^

H6: Sub-sub-sub-section
"""""""""""""""""""""""


TEXT
====

*italic*

(no spaces allowed between asterix and word - i.e. *italic* works but * italic* does not)

(partial is ok after a backslash - i.e. *longish* gives "longish" in italics - long\ *ish* gives "longish" with the 'ish' italicicized)

**bold**

``verbatim``

hyperlink `Google <www.google.com>`_


DIRECTIVES
==========

.. <name>:: <arguments>
    :<option>: <option values>

    content


INTERNAL LINKS
==============

`Internal and external links`_

`TEXT`_

`H1: Titles/Parts`_


TABLE WITH HEADERS
==================

+------------+------------+-----------+
| Header 1   | Header 2   | Header 3  |
+============+============+===========+
| body row 1 | column 2   | column 3  |
+------------+------------+-----------+
| body row 2 | Cells may span columns.|
+------------+------------+-----------+
| body row 3 | Cells may  | - Cells   |
+------------+ span rows. | - contain |
| body row 4 |            | - blocks. |
+------------+------------+-----------+



TODO
====

.. 
   [TODO] To do can be used if surrounded by brackets
