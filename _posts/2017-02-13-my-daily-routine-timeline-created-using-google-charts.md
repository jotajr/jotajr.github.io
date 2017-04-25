---
layout: post
title: My daily routine timeline created using Google Charts
description: Creating my responsive daily routine timeline using Google Charts in just few lines of HTML and JavaScript.
keywords: my daily routine, google timeline chart, responsive timeline chart
tags: [Google Charts, JavaScript]
comments: true
---

<script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
<div id="timeline" style="height: 150px;"></div>
<script>
google.charts.load('current', {
  'packages': ['timeline']
});
google.charts.setOnLoadCallback(drawChart);

function drawChart() {
  var container = document.getElementById('timeline');
  var chart = new google.visualization.Timeline(container);
  var dataTable = new google.visualization.DataTable();

  dataTable.addColumn({ type: 'string', id: 'day' });
  dataTable.addColumn({ type: 'string', id: 'Routine' });
  dataTable.addColumn({ type: 'date', id: 'Start' });
  dataTable.addColumn({ type: 'date', id: 'End' });

  dataTable.addRows([
    ['Weekday', 'Sleep', new Date(0, 0, 0, 0, 0, 0), new Date(0, 0, 0, 6, 0, 0)],
    ['Weekday', 'Misc.', new Date(0, 0, 0, 6, 0, 0), new Date(0, 0, 0, 8, 0, 0)],
    ['Weekday', 'Drive', new Date(0, 0, 0, 8, 0, 0), new Date(0, 0, 0, 8, 30, 0)],
    ['Weekday', 'Breakfast', new Date(0, 0, 0, 8, 30, 0), new Date(0, 0, 0, 9, 0, 0)],
    ['Weekday', 'Work', new Date(0, 0, 0, 9, 0, 0), new Date(0, 0, 0, 12, 0, 0)],
    ['Weekday', 'Lunch', new Date(0, 0, 0, 12, 0, 0), new Date(0, 0, 0, 13, 0, 0)],
    ['Weekday', 'Work', new Date(0, 0, 0, 13, 0, 0), new Date(0, 0, 0, 17, 0, 0)],
    ['Weekday', 'Drive', new Date(0, 0, 0, 17, 0, 0), new Date(0, 0, 0, 17, 45, 0)],
    ['Weekday', 'Misc.', new Date(0, 0, 0, 17, 45, 0), new Date(0, 0, 0, 20, 0, 0)],
    ['Weekday', 'Dinner', new Date(0, 0, 0, 20, 0, 0), new Date(0, 0, 0, 20, 30, 0)],
    ['Weekday', 'Misc.', new Date(0, 0, 0, 20, 30, 0), new Date(0, 0, 0, 21, 0, 0)],
    ['Weekday', 'Creative', new Date(0, 0, 0, 21, 0, 0), new Date(0, 0, 0, 24, 00, 0)],

    ['Weekend', 'Creative', new Date(0, 0, 0, 0, 0, 0), new Date(0, 0, 0, 6, 0, 0)],
    ['Weekend', 'Misc.', new Date(0, 0, 0, 6, 0, 0), new Date(0, 0, 0, 7, 0, 0)],
    ['Weekend', 'Sleep', new Date(0, 0, 0, 7, 0, 0), new Date(0, 0, 0, 13, 0, 0)],
    ['Weekend', 'Lunch', new Date(0, 0, 0, 13, 0, 0), new Date(0, 0, 0, 13, 30, 0)],
    ['Weekend', 'Family Moment', new Date(0, 0, 0, 13, 30, 0), new Date(0, 0, 0, 17, 0, 0)],
    ['Weekend', 'Sport', new Date(0, 0, 0, 17, 0, 0), new Date(0, 0, 0, 18, 30, 0)],
    ['Weekend', 'Misc.', new Date(0, 0, 0, 18, 30, 0), new Date(0, 0, 0, 20, 0, 0)],
    ['Weekend', 'Dinner', new Date(0, 0, 0, 20, 0, 0), new Date(0, 0, 0, 20, 30, 0)],
    ['Weekend', 'Misc.', new Date(0, 0, 0, 20, 30, 0), new Date(0, 0, 0, 21, 0, 0)],
    ['Weekend', 'Gaming', new Date(0, 0, 0, 21, 0, 0), new Date(0, 0, 0, 24, 0, 0)],
  ]);

  var options = {
    timeline: {
      showRowLabels: true,
      avoidOverlappingGridLines: false
    }
  };

  chart.draw(dataTable, options);

  var addEvent = function(object, type, callback) {
    if (object == null || typeof(object) == 'undefined') return;
    if (object.addEventListener) {
      object.addEventListener(type, callback, false);
    } else if (object.attachEvent) {
      object.attachEvent("on" + type, callback);
    } else {
      object["on" + type] = callback;
    }
  };

  addEvent(window, "resize", function(event) {
    chart.draw(dataTable, options);
  });

}
</script>

This is my responsive daily routine timeline created in JavaScript using [Google Charts](https://developers.google.com/chart/interactive/docs/). Based on that routine timeline, well it looked like my weekend life is not really normal anymore. Truly speaking, I love to create stuffs (Creative label) during those hours which causes my sleep time shifted. However, this routine timeline is not always true, but it is fun to experiment with the "timeline" idea.

Here's the source code:

```html
<script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
<div id="timeline" style="height: 150px;"></div>
```

```js
google.charts.load('current', {
  'packages': ['timeline']
});
google.charts.setOnLoadCallback(drawChart);

function drawChart() {
  var container = document.getElementById('timeline');
  var chart = new google.visualization.Timeline(container);
  var dataTable = new google.visualization.DataTable();

  dataTable.addColumn({ type: 'string', id: 'day' });
  dataTable.addColumn({ type: 'string', id: 'Routine' });
  dataTable.addColumn({ type: 'date', id: 'Start' });
  dataTable.addColumn({ type: 'date', id: 'End' });

  dataTable.addRows([
    ['Weekday', 'Sleep', new Date(0, 0, 0, 0, 0, 0), new Date(0, 0, 0, 6, 0, 0)],
    ['Weekday', 'Misc.', new Date(0, 0, 0, 6, 0, 0), new Date(0, 0, 0, 8, 0, 0)],
    ['Weekday', 'Drive', new Date(0, 0, 0, 8, 0, 0), new Date(0, 0, 0, 8, 30, 0)],
    ['Weekday', 'Breakfast', new Date(0, 0, 0, 8, 30, 0), new Date(0, 0, 0, 9, 0, 0)],
    ['Weekday', 'Work', new Date(0, 0, 0, 9, 0, 0), new Date(0, 0, 0, 12, 0, 0)],
    ['Weekday', 'Lunch', new Date(0, 0, 0, 12, 0, 0), new Date(0, 0, 0, 13, 0, 0)],
    ['Weekday', 'Work', new Date(0, 0, 0, 13, 0, 0), new Date(0, 0, 0, 17, 0, 0)],
    ['Weekday', 'Drive', new Date(0, 0, 0, 17, 0, 0), new Date(0, 0, 0, 17, 45, 0)],
    ['Weekday', 'Misc.', new Date(0, 0, 0, 17, 45, 0), new Date(0, 0, 0, 20, 0, 0)],
    ['Weekday', 'Dinner', new Date(0, 0, 0, 20, 0, 0), new Date(0, 0, 0, 20, 30, 0)],
    ['Weekday', 'Misc.', new Date(0, 0, 0, 20, 30, 0), new Date(0, 0, 0, 21, 0, 0)],
    ['Weekday', 'Creative', new Date(0, 0, 0, 21, 0, 0), new Date(0, 0, 0, 24, 00, 0)],

    ['Weekend', 'Creative', new Date(0, 0, 0, 0, 0, 0), new Date(0, 0, 0, 6, 0, 0)],
    ['Weekend', 'Misc.', new Date(0, 0, 0, 6, 0, 0), new Date(0, 0, 0, 7, 0, 0)],
    ['Weekend', 'Sleep', new Date(0, 0, 0, 7, 0, 0), new Date(0, 0, 0, 13, 0, 0)],
    ['Weekend', 'Lunch', new Date(0, 0, 0, 13, 0, 0), new Date(0, 0, 0, 13, 30, 0)],
    ['Weekend', 'Family Moment', new Date(0, 0, 0, 13, 30, 0), new Date(0, 0, 0, 17, 0, 0)],
    ['Weekend', 'Sport', new Date(0, 0, 0, 17, 0, 0), new Date(0, 0, 0, 18, 30, 0)],
    ['Weekend', 'Misc.', new Date(0, 0, 0, 18, 30, 0), new Date(0, 0, 0, 20, 0, 0)],
    ['Weekend', 'Dinner', new Date(0, 0, 0, 20, 0, 0), new Date(0, 0, 0, 20, 30, 0)],
    ['Weekend', 'Misc.', new Date(0, 0, 0, 20, 30, 0), new Date(0, 0, 0, 21, 0, 0)],
    ['Weekend', 'Gaming', new Date(0, 0, 0, 21, 0, 0), new Date(0, 0, 0, 24, 0, 0)],
  ]);

  var options = {
    timeline: {
      showRowLabels: true,
      avoidOverlappingGridLines: false
    }
  };

  chart.draw(dataTable, options);

  var addEvent = function(object, type, callback) {
    if (object == null || typeof(object) == 'undefined') return;
    if (object.addEventListener) {
      object.addEventListener(type, callback, false);
    } else if (object.attachEvent) {
      object.attachEvent("on" + type, callback);
    } else {
      object["on" + type] = callback;
    }
  };

  addEvent(window, "resize", function(event) {
    chart.draw(dataTable, options);
  });

}
```
