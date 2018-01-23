# HighchartsXAxisSpecificationProblem
Three example Highstocks HTML files, one works, one is broken by making the data timestamps irregular, and the last shows a failed attempt to fix

### Background
The working version is a slightly modified version of the example Emerson entered for help with a javascript console error 15 (https://stackoverflow.com/questions/46164090/sorting-scatter-highstock-chart-with-multiple-series). Ignoring the console error, we want to the Highstocks navigator on a scatter plot with *irregular* data timestamps. The working version included here has a large 2D-array 'points' with regular time intervals. The xaxis declaration has a 'data' definition mapping values from 'points'.

    data: points.map(function(point) {
      return [point[2]];
    }),

the only changes in the broken version is a set of arbitrary deletes from the 'points' array to force the timestamps to be sufficiently irregular to break the date inference provided by Highcharts. (If you delete just a few lines from the working copy's 'points' 2D-array, it still works). In the screen grab 'broken_badTickArray.html.png', you can see the dates are Dec 31, 1969 to Jan 1, 1970 and the tick array data is indecipherable.

### The attempted fix (version uploaded only 'representative' of several tries)
The screen grab 'works_goodTickArray.html.png' shows that Highcharts boiled down the large number of timestamps to a small set of midnight day boundaries. In the attempted fix version, the following code generators an explicit set of timestamps that are then given as the value for the x-axis data.

	var xtd = [];
	var apr222017 = 1492819200000;
	var may162017 = 1494892800000;
	var x = 0;
	do {
		xtd[x] = apr222017 + (x * 86400000);
		x++
	} while ((apr222017 + (x * 86400000)) < may162017);
  
  When that didn't work, attempted to set 'floor' and 'ceiling'...
  
    // ...
    data: xtd,
    floor: apr222017,
    ceiling: may162017,
    // ...
    
  Having no luck with that added...
  
    min: apr222017,
    max: may162017,
  
  which didn't help. Nor did removng the floor and ceiling definitions and going only with min/max.
  
  Adding the following also failed:
      tickPositioner: function() {
        return xtd;
      },

