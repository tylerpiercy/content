<h1>Indexing a List with VLOOKUP</h1>
<p>When writing formulas, we often encounter cases where one or more of the inputs to the function will depend on the value of another input. More specifically, we need to use one of the inputs to lookup the other input from a table. This can be easily accomplished using the VLOOKUP function. </p>
<p>For example, the following workbook computes the volume and weight of a set of cylinders. The weight is computing from the volume and the unit weight. However, the unit weight depends on the material being used. Unit weights for a set of common materials are shown in a table at the top:</p>
<p><img src="start.png" width="473" height="609" alt=""/></p>
<p>The objective of this exercise is to determine the appropriate unit weight for each cylinder and calculate the correct weight by multiplying the selected unit weight by the computed volume. We will do this by automatically selecting the correct unit weight from the list using the VLOOKUP function.</p>
<h2>Data Validation</h2>
<p>Before using the VLOOKUP function, we need to enter a set of materials in the Material column. We need to be careful as we do this because due to the way the VLOOKUP function works, we need to ensure that the items in the Materials column are spelled exactly the same way they are spelled in the unit weight table at the top. We can do this with a tool called <strong>Data Validation</strong>. This process is described in the <a href="../excel-validation/index.php">Data Validation</a> chapter.</p>
<h2>VLOOKUP Function</h2>
<p>Once the material values are entered in column E, we are ready to use the VLOOKUP function. The syntax for the VLOOKUP function is as follows:</p>
<blockquote>
  <p>VLOOKUP(lookup_value,table_array,col_index_num,range_lookup) </p>
</blockquote>
<p>where:</p>
<blockquote>
  <table width="433" border="1">
    <tr>
      <td width="119">Lookup_value</td>
      <td width="298">The value to be found in the first column of the array</td>
    </tr>
    <tr>
      <td>Table_array</td>
      <td>The table of information in which data is looked up. Use a reference to a range or a range name</td>
    </tr>
    <tr>
      <td>Col_index_num</td>
      <td>The column number in table_array from which the matching value must be returned</td>
    </tr>
    <tr>
      <td>Range_lookup</td>
      <td>A logical value (TRUE or FALSE) that specifies whether you want VLOOKUP to find an exact match or an approximate match. Explained in more detail below.</td>
    </tr>
  </table>
</blockquote>
<p>So for our case, we will use VLOOKUP to select a unit weight value from the table using the user-specified material. The unit weight returned by the function is then multiplied by the volume to compute the cylinder weight as follows:</p>
<p><img src="vlookup-1.png" width="653" height="351" alt=""/></p>
<p>The first argument (E13) to the VLOOKUP function refers to the Material value on the same row and is a relative reference. The second argument ($B$5:$C$10) is an absolute reference to the table use for the lookup. The lookup value (&quot;Concrete&quot; in this case) is used to search through the first column in the table to find the row matching the lookup value. In this case, the match is found on the third row of the table (cell B7). The third argument (&quot;2&quot;) tells the VLOOKUP function from which column of the table the return value should be selected. Since the value is 2, we go to the second column of the lookup table on the selected row and find our value (&quot;150&quot;). This is the value that is returned by the function and multiplied by the volume (&quot;1.6&quot;) to compute the weight. After copying this formula to the rest of the column, the weight values are all correctly computed as follows:</p>
<p><img src="vlookup-1b.png" width="457" height="353" alt=""/></p>
<p>If the the values in the lookup table are edited, all of the weights would be automatically updated.</p>
<h2>Range Lookups</h2>
<p>In the example shown in the previous section, we are doing an exact match on the lookup value in the first column. In some cases we are not looking for an exact match, but we need to find a match from a set of numerical ranges. For example, suppose that we wanted to categorize the cylinder weights using the following guidelines:</p>
<blockquote>
  <table width="309" border="1">
    <tr>
      <td width="160" align="center"><strong>Range</strong></td>
      <td width="133" align="center"><strong>Category</strong></td>
    </tr>
    <tr>
      <td align="center">wt &le; 1000</td>
      <td align="center">Ultra Light</td>
    </tr>
    <tr>
      <td align="center">1000 &le; wt &le; 2000</td>
      <td align="center">Light</td>
    </tr>
    <tr>
      <td align="center">2000 &le; wt &le; 10,000</td>
      <td align="center">Medium</td>
    </tr>
    <tr>
      <td align="center">10,000&le; wt &le; 100,000</td>
      <td align="center">Heavy</td>
    </tr>
    <tr>
      <td align="center">100,000 &le; wt</td>
      <td align="center">Extra Heavy</td>
    </tr>
  </table>
</blockquote>
<p>We will then add a new table and an extra column as follows:</p>
<p><img src="rangelookup-1.png" width="579" height="615" alt=""/></p>
<p>Note that the weight values in the first column of the weight-category table at the top right has been sorted in ascending order. This is critical in order for the lookup to work. Next, we enter a formula using the VLOOKUP function as follows:</p>
<p><img src="rangelookup-2.png" width="687" height="337" alt=""/></p>
<p>Notice that the last argument (range_lookup) has a value of <strong>TRUE</strong>. This means that we take the lookup_value (&quot;235.6&quot; in this case) and we look through the first column of the table until we find a row where the value on the row is less than or equal to the lookup_value and the value on the next row is greater than the lookup_value. In this case, the match occurs on the first row and so the resulting value from column 2 is &quot;Ultra Light&quot;. After copying the formula to the rest of the Category column, the resulting values are as follows:</p>
<p><img src="rangelookup-3.png" width="320" height="524" alt=""/></p>
<p>It is important to note that the range_lookup argument to the VLOOKUP function is optional. If it is omitted, it is assumed to be TRUE by default. A common error with the VLOOKUP function is to omit this argument when the VLOOKUP function is intended to be used as an exact match. This can lead to unintended errors, depending on how the values in the first column are ordered. Therefore, it is strongly recommended to always enter a TRUE or FALSE value for the range_lookup argument every time the VLOOKUP function is used.</p>
<h2>Two-Dimensional Lookup</h2>
<p>Occasionally it is useful to do a two-dimensional lookup where a value is found from a table containing both rows and columns. For example, consider the following sheet containing a table of temperatures in degree F illustrating a relationship between average monthly temp and elevation in ft for a particular location.</p>
<p><img src="tempelev.png" width="876" height="706" alt=""/></p>
<p>Starting at row 24, another table is listed and the objective is to fill in the <strong>Temp</strong> column with a formula that looks up the temp corresponding to the elevation from column <strong>C</strong> and the month associated with the date provided in column <strong>B</strong>. This requires a double lookup. We use VLOOKUP to find the row we need based on a range lookup of elevation using the VLOOKUP function. Then, for the third argument to VLOOKUP, we need to determine which column to use based on the month desired. To find the right column based on the month, we first need to find the month label (&quot;Jan&quot;, &quot;Feb&quot;, etc.) from a date value. This can  be accomplished using the <strong>TEXT</strong> function which takes a date as an argument and returns the month or day value depending on the format specified by the second argument as follows:</p>
<blockquote>
  <p>TEXT(B24,&quot;MMM&quot;)</p>
</blockquote>
<p>For the values shown, the function would return &quot;<strong>Mar</strong>&quot;. Then we need to use this text string to automatically find the index of the column corresponding to this month. This can be done with the <strong>MATCH</strong> function as follows:</p>
<blockquote>
  <p>MATCH(TEXT(B24,&quot;MMM&quot;),$C$5:$N$5,0)</p>
</blockquote>
<p>The first argument to the MATCH funciton is the lookup value, the second argument is an array (row or column of values) and the third argument indicates the type of match to perform (a value of <strong>0</strong> tells it to find an exact match). The function looks through the array to find the lookup value and returns the index of the item if found. For the arguments shown, the function would return a value of <strong>3</strong>. At this point, we are ready to use the VLOOKUP function. We would formulate the function call as follows:</p>
<p><img src="tempelev2.png" width="879" height="704" alt=""/></p>
<p>Note that  we are using a range lookup on elevation so the last argument to VLOOKUP is <strong>TRUE</strong>.</p>
