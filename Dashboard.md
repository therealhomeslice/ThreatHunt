
Place for me to store 

<form> 

<label>Hunt Dashboard</label> 

<fieldset submitButton="false"> 

<input type="time" token="time_pick" searchWhenChanged="true"> 

<label>Time Selector</label> 

<default> 

<earliest>-24h@h</earliest> 

<latest>now</latest> 

</default> 

</input> 

</fieldset> 

<row> 

<panel> 

<title>Unique Network Connections</title> 

<chart> 

<search> 

<query>| pivot Hunt_Dashboard RootObject dc(RemoteAddress) AS "Distinct Count of RemoteAddress" SPLITROW RemoteAddress AS RemoteAddress FILTER RemoteAddress isNot 0.0.0 <earliest>$time_pick.earliest$</earliest> 

<latest>$time_pick.latest$</latest> 

<sampleRatio>1</sampleRatio> 

</search> 

<option name="charting.axisLabelsX.majorLabelStyle.overflowMode">ellipsisNone</option> 

<option name="charting.axisLabelsX.majorLabelStyle.rotation">0</option> 

<option name="charting.axisTitleX.visibility">visible</option> 

<option name="charting.axisTitleY.visibility">visible</option> 

<option name="charting.axisTitleY2.visibility">visible</option> 

<option name="charting.axisX.abbreviation">none</option> 

<option name="charting.axisX.scale">linear</option> 

<option name="charting.axisY.abbreviation">none</option> 

<option name="charting.axisY.scale">linear</option> 

<option name="charting.axisY2.abbreviation">none</option> 

<option name="charting.axisY2.enabled">0</option> 

<option name="charting.axisY2.scale">inherit</option> 

<option name="charting.chart">pie</option> 

<option name="charting.chart.bubbleMaximumSize">50</option> 

<option name="charting.chart.bubbleMinimumSize">10</option> 

<option name="charting.chart.bubbleSizeBy">area</option> 

<option name="charting.chart.nullValueMode">gaps</option> 

<option name="charting.chart.showDataLabels">none</option> 

<option name="charting.chart.sliceCollapsingThreshold">0.01</option> 

<option name="charting.chart.stackMode">default</option> 

<option name="charting.chart.style">shiny</option> 

<option name="charting.drilldown">none</option> 

<option name="charting.layout.splitSeries">0</option> 

<option name="charting.layout.splitSeries.allowIndependentYRanges">0</option> 

<option name="charting.legend.labelStyle.overflowMode">ellipsisMiddle</option> 

<option name="charting.legend.mode">standard</option> 

<option name="charting.legend.placement">right</option> 

<option name="charting.lineWidth">2</option> 

<option name="refresh.display">progressbar</option> 

<option name="trellis.enabled">0</option> 

<option name="trellis.scales.shared">1</option> 

<option name="trellis.size">medium</option> 

</chart> 

</panel> 

</row> 

<row> 

<panel> 

<title>User Logins by Time</title> 

<chart> 

<search> 

<query>| pivot Logins_per_time RootObject dc(host) AS "Distinct Count of host" SPLITROW _time AS _time PERIOD auto SPLITCOL UserName SORT 0 _time ROWSUMMARY 0 COLSUMMARY <earliest>$time_pick.earliest$</earliest> 

<latest>$time_pick.latest$</latest> 

<sampleRatio>1</sampleRatio> 

</search> 

<option name="charting.axisLabelsX.majorLabelStyle.overflowMode">ellipsisNone</option> 

<option name="charting.axisLabelsX.majorLabelStyle.rotation">0</option> 

<option name="charting.axisLabelsY.majorUnit">1</option> 

<option name="charting.axisTitleX.visibility">visible</option> 

<option name="charting.axisTitleY.text">Distinct Count of host</option> 

<option name="charting.axisTitleY.visibility">visible</option> 

<option name="charting.axisTitleY2.visibility">visible</option> 

<option name="charting.axisX.abbreviation">none</option> 

<option name="charting.axisX.scale">linear</option> 

<option name="charting.axisY.abbreviation">none</option> 

<option name="charting.axisY.scale">linear</option> 

<option name="charting.axisY2.abbreviation">none</option>

<option name="charting.axisY2.enabled">0</option> 

<option name="charting.axisY2.scale">inherit</option> 

<option name="charting.chart">line</option> 

<option name="charting.chart.bubbleMaximumSize">50</option> 

<option name="charting.chart.bubbleMinimumSize">10</option> 

<option name="charting.chart.bubbleSizeBy">area</option> 

<option name="charting.chart.nullValueMode">gaps</option> 

<option name="charting.chart.showDataLabels">none</option> 

<option name="charting.chart.sliceCollapsingThreshold">0.01</option> 

<option name="charting.chart.stackMode">default</option> 

<option name="charting.chart.style">shiny</option> 

<option name="charting.drilldown">none</option> 

<option name="charting.layout.splitSeries">0</option> 

<option name="charting.layout.splitSeries.allowIndependentYRanges">0</option> 

<option name="charting.legend.labelStyle.overflowMode">ellipsisMiddle</option> 

<option name="charting.legend.mode">standard</option> 

<option name="charting.legend.placement">right</option> 

<option name="charting.lineWidth">2</option> 

<option name="trellis.enabled">0</option> 

<option name="trellis.scales.shared">1</option> 

<option name="trellis.size">medium</option> 

</chart> 

</panel> 

</row> 

<row> 

<panel> 

<title>Unique Logins</title> 

<table> 

<search> 

<query>| pivot Unique_logins RootObject values(host) AS "Distinct Values of host" SPLITROW UserName AS UserName FILTER HomeDirectory isNot Null SORT 100 UserName ROWSUMM <earliest>$time_pick.earliest$</earliest> 

<latest>$time_pick.latest$</latest> 

<sampleRatio>1</sampleRatio> 

</search> 

<option name="count">20</option> 

<option name="dataOverlayMode">none</option> 

<option name="drilldown">none</option> 

<option name="percentagesRow">false</option> 

<option name="refresh.display">progressbar</option> 

<option name="rowNumbers">false</option> 

<option name="totalsRow">false</option> 

<option name="wrap">true</option> 

</table> 

</panel> 

</row> 

</form> 
