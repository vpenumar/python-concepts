import pandas as pd
import numpy as np
from bokeh.io import curdoc
from bokeh.plotting import figure
from bokeh.models import ColumnDataSource,Select
from bokeh.layouts import widgetbox, column
df = pd.read_csv('https://raw.githubusercontent.com/vpenumar/Scientific-visualization/master/CleanLabels.csv', encoding = 'ISO-8859-1', header=[0,1])

df2 = df.iloc[:,1:len(df.columns):2]
print(df.info())
select = Select(title='Data Set', options = ['A','B','C','D','E','F','G','H','I','J','K','L','M'],
                value= 'A')
source = ColumnDataSource(data={'x': df[select.value]['x'],
                                'y': df[select.value]['y']})
plot = figure(title = 'Data Dozen')
plot.circle(x='x',y='y',source = source)

def update_plot(attr,old,new):
    source.data = {'x': df[select.value]['x'],
                   'y': df[select.value]['y']}

select.on_change('value',update_plot)
layout= column(widgetbox(select),plot)
# Add the layout to the current document
curdoc().add_root(layout)