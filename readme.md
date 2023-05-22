# Responsive Tables

## HTML

Use a `<table>` for accessibility.
Use a `<caption>` as the table's first child to give the table a name.
To easily read which value refers to which category:

- Use comment on each `<td>`:
  ```html
  <td>Max Verstappen</td>
  <!-- name -->
  ```
- Use data attributes for each `<td>`:
  ```html
  <td data-cell="name">Max Verstappen</td>
  ```

## CSS

You generally want to remove the default border with `border-collapse: collapse;`. ⚠ It removes padding though so you have to add it elsewhere.

```css
table {
	border-collapse: collapse;
}

caption,
th,
td {
	padding: 1rem;
}
```

Left-align the `<caption>` and `<th>` text:

```css
caption,
th {
	text-align: left;
}
```

### Improving the look

Table width, which adds responsiveness as well:

```css
table {
	width: 100%;
	border-collapse: collapse;
}
```

Different background color for the `<th>`

```css
th {
	background: hsl(0 0% 0% / 0.5);
}
```

**Zebra Pattern**: To track data better, add a slightly different background color for each even or odd row:

```css
tr:nth-of-type(2n) {
	background: hsl(0 0% 0% / 0.1);
}
```

If you ever want to style the caption as well as the table, you must add its selector to the CSS:

```css
table,
caption {
	/* weird example, do not use exactly this */
	background: white;
	color: black;
}
```

## Making it responsive

We already made width 100% earlier:

```css
table {
	width: 100%;
}
```

First step result:
![[responsive_tables_mobile.png]]

The complexity is for smaller screen sizes, so we'll use a desktop first mindset and add a media query for smaller sizes:

```css
@media (max-width: 650px) {
	/* removing the first row */
	th {
		display: none;
	}

	/* getting all the data of a row in a block */
	td {
		display: block;
		padding: 0.4rem 1rem;
	}

	/* getting more space on top of the block */
	td:first-child {
		padding-top: 2rem;
	}

	/* getting more space on the bottom */
	td:last-child {
		padding-bottom: 2rem;
	}

	/* displaying category titles in front of each "cell" */
	td::before {
		content: attr(data-cell) ': ';
		font-weight: 700;
		text-transform: capitalize;
	}
}
```

### Improving the layout with `grid`

```css
td {
	display: grid;
	gap: 0.5rem;
	grid-template-columns: 15ch auto;
	padding: 0.4rem 1rem;
}
```

![[responsive_tables_grid_layout.png]]

## Fixing the accessibility

Using `display: block` or `display: grid`, we destroyed the semantics 😟

Fortunately, we can use JS with the help of [Adrian Roselli](https://adrianroselli.com/2018/05/functions-to-add-aria-to-tables-and-lists.html).

```js
function AddTableARIA() {
	try {
		var allTables = document.querySelectorAll('table')
		for (var i = 0; i < allTables.length; i++) {
			allTables[i].setAttribute('role', 'table')
		}
		var allCaptions = document.querySelectorAll('caption')
		for (var i = 0; i < allCaptions.length; i++) {
			allCaptions[i].setAttribute('role', 'caption')
		}
		var allRowGroups = document.querySelectorAll('thead, tbody, tfoot')
		for (var i = 0; i < allRowGroups.length; i++) {
			allRowGroups[i].setAttribute('role', 'rowgroup')
		}
		var allRows = document.querySelectorAll('tr')
		for (var i = 0; i < allRows.length; i++) {
			allRows[i].setAttribute('role', 'row')
		}
		var allCells = document.querySelectorAll('td')
		for (var i = 0; i < allCells.length; i++) {
			allCells[i].setAttribute('role', 'cell')
		}
		var allHeaders = document.querySelectorAll('th')
		for (var i = 0; i < allHeaders.length; i++) {
			allHeaders[i].setAttribute('role', 'columnheader')
		}
		// this accounts for scoped row headers
		var allRowHeaders = document.querySelectorAll('th[scope=row]')
		for (var i = 0; i < allRowHeaders.length; i++) {
			allRowHeaders[i].setAttribute('role', 'rowheader')
		}
	} catch (e) {
		console.log('AddTableARIA(): ' + e)
	}
}

AddTableARIA()
```

## Resources

- [Kevin Powell's video on YT](https://www.youtube.com/watch?v=czZ1PvNW5hk)
- [Adrian Roselli - A Responsive Accessible Table](https://adrianroselli.com/2017/11/a-responsive-accessible-table.html#ResponsiveViewportWidthHeaders)
- [Adrian Roselli - JS Functions to Add ARIA to Tables and Lists](https://adrianroselli.com/2018/05/functions-to-add-aria-to-tables-and-lists.html)
