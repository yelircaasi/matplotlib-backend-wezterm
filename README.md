# matplotlib-backend-wezterm

This python module allows you to use your
[wezterm terminal](https://wezfurlong.org/wezterm/index.html)
to show the plots generated by python's
[matplotlib](https://github.com/matplotlib/matplotlib).

For other terminals, the similar [notcurses backend](https://github.com/jktr/matplotlib-backend-notcurses)
may also be of interest.

To install this one, do one of the following

 - `$ pip install --user matplotlib-backend-wezterm`
 - clone this repo into your python's `site-packages` directory
 - clone this repo and add the parent directory to `sys.path` or `$PYTHONPATH`

Then, configure matplotlib to use the module by either setting the
environment variable `MPLBACKEND` to `module://matplotlib-backend-wezterm`
or by initializing matplotlib as follows.

```python
import matplotlib
matplotlib.use('module://matplotlib-backend-wezterm')
import matplotlib.pyplot as plt
```

If you've installed this module correctly, you can now use
the following sample code to draw a plot in your terminal.

```
$ export MPLBACKEND='module://matplotlib-backend-wezterm'
$ python -i
>>> n = 10000
>>> df = pd.DataFrame({'x': np.random.randn(n),
                       'y': np.random.randn(n)})
>>> df.plot.hexbin(x='x', y='y', gridsize=20)
<plot is shown>
```

If you set your matplotlib to interactive mode via
`matplotlib.pyplot.ion()` or by running python as
`python -i`, non-empty figures are drawn on construction
where possible. This allows you to use pandas' `plot()`
calls directly, without calling `plt.show()`, and still
enables you to manually construct and `plt.show()`.

If your matplotlib is in non-interactive mode,
you can construct your figures as usual, and then call
`plt.show()` to render them to your terminal. This
works from both a repl and when running scripts.

Figures are resized to the size of your terminal by default.
If you'd rather control the sizing of figures manually,
set the `MPLBACKEND_WEZTERM_SIZING` environment variable to `manual`.

Internally, this backend is somewhat based on matplotlib's
IPython support: it's a hybrid of image and GUI backend types.
It works by using matplotlib's `Agg` backend to render the
plot, and then calls wezterm's `icat` to place the rendered
image on your terminal. This means that plotting works as
expected, but the image drawn to your terminal isn't
interactive and animations aren't supported.
