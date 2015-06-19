Spaghetti à la Hivos
====================

Show the network of Hivos partner organisations, with an impression of the
budgets they work with.

You will need:

 * [Open Refine][openrefine] a tool to manipulate data.

Begin with the end in mind
--------------------------

Think of what you’d like to show, what kind of data do you need to make that
possible and check if you have the right data.

Get your ingredients (the data)
-------------------------------

### Set up Open Refine

1. Go to the IATI Registry and find and copy the link to the Hivos IATI file.
1. Start your Open Refine: it will open in your browser. Select "Create a new project"
1. Choose "Web address" and paste the link to the Hivos file.
  (Hivos has a quirk: the actual link should start with **https** instead of **http**)
1. Next, select <code>iati-activity</code> as the element to process.
  ![hivos-1]
1. Then click the **Create project** button in the top-right corner.

[openrefine]: http://openrefine.org/download.html
[hivos-1]: hivos-1.png

### Analyse the data

Let’s analyse the data Hivos has included in their IATI file. You can easily do this using facets and filters.

* Countries

  1. At the top of each column you’ll find an arrow pointing downward that offers
you a range of powerful options to analyse and edit the data in that column.

    Go to the column at the end, with the header
    <code>iati-activity - recipient-country</code>

    Choose the menu _Facet > Text facet_

  1. On the left-hand side, you’ll now see a box showing all the different values
in this column.

    - There are many different countries in the list.
    - There are a couple of "Regional: ..." entries.
    - There are "(blank)" entries.

* Budgets

  1. Towards the end there also is a column
  <code>iati-activity - budget - value - currency</code>

    Again, choose the menu _Facet > Text facet_

  1. On the left-hand side, you'll see different currencies.

### Shape and clean the data

Now we're going to shape the data, using our power tool [Open Refine][openrefine].

1. Find the column <code>iati-activity - iati-identifier</code>, click the
arrow and select _Edit column > Move column to beginning_
1. Notice how in the top-left corner the _Undo / Redo_ tab now shows a **1**
<br>![hivos-2]
1. When you click the tab, you’ll see all the _transformations_ you’ve made so
far. Here you can easily undo and redo your steps. Feel free to play around,
showing or hiding columns, changing things!
1. Also notice the buttons _Extract_ and _Apply_. Here you have access to the
code that Open Refine produced to execute your transformation. You can copy this
code and use it in future projects.

[hivos-2]: hivos-2.png

To follow the recipe:

1. Go back to the original data by clicking on “0. create project”. The columns
you removed or changed are back again.
1. Now click the “Apply...” button, and copy and paste the code below:
  * we'll remove most of the columns (we don't need them)

  <pre>
  [
    {
      "op": "core/column-reorder",
      "description": "Reorder columns",
      "columnNames": [
        "iati-activity - iati-identifier",
        "iati-activity - title",
        "iati-activity - sector",
        "iati-activity - sector - code",
        "iati-activity - sector - percentage",
        "iati-activity - participating-org",
        "iati-activity - participating-org - role",
        "iati-activity - budget - value",
        "iati-activity - budget - value - currency",
        "iati-activity - recipient-country",
        "iati-activity - recipient-country - code"
      ]
    }
  ]
</pre>

We're not interested in funding partners, just in implementing partners, so we
choose to select and remove funding partners now:

1. Under _iati-activity - participating-org - role_, select "Funding" and then
remove all matching rows.
<br>![hivos-3]

[hivos-3]: hivos-3.png

1. Or in an _Apply..._ phrase:

  <pre>
  [
    {
      "op": "core/row-removal",
      "description": "Remove rows",
      "engineConfig": {
        "facets": [
          {
            "invert": false,
            "expression": "value",
            "selectError": false,
            "omitError": false,
            "selectBlank": false,
            "name": "iati-activity - participating-org - role",
            "omitBlank": false,
            "columnName": "iati-activity - participating-org - role",
            "type": "list",
            "selection": [
              {
                "v": {
                  "v": "Funding",
                  "l": "Funding"
                }
              }
            ]
          }
        ],
        "mode": "record-based"
      }
    }
  ]
  </pre>
