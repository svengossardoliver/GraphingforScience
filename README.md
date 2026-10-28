[README.md](https://github.com/user-attachments/files/34068687/README.md)
# Strothoff Graphical Laboratories

A graphing and data-processing tool for IB Diploma Group 4 sciences (physics, chemistry, biology). Students enter (x, y) data with uncertainties and get a graph ready for a lab report: a best-fit model, R², rigorous max/min lines and properly formatted data tables.

Created by Christopher B. Slough, Head of Science, Strothoff International School.
© 2026 Christopher B. Slough.

---

## Features

**Data entry**
- Quantity, symbol, unit and uncertainty for each axis. Uncertainties can be absolute (± 0.01) or a percentage (± 2 %).
- 1–6 repeated trials per point. Each point is the mean, and its uncertainty is half the range of the trials or the instrument uncertainty, whichever is larger.
- Optional per-point uncertainties.
- Values are stored exactly as typed, so `1.00` stays `1.00`. A decimal comma (`1,23`) is accepted.
- Columns can be pasted from a spreadsheet. **Enter** and the arrow keys move between cells.
- **Strict precision mode** (on by default) requires every value to be written to the same decimal place as its uncertainty. Inconsistent cells are highlighted with the correct form, and the graph will not plot until they are fixed.

**Tables**
- *Raw data* table with IB-style headings: `Length L / m ± 0.002`.
- *Processed data* table with means, absolute uncertainties and linearised values. Each value is rounded to the decimal place of its uncertainty.
- Either table can be copied into Word or Google Docs as a real table, or saved as PNG or CSV.

**Graph**
- Six fits: linear, proportional, quadratic, power, exponential, logarithmic. The equation and R² are shown on the graph.
- Linearisation: plot x or y as x², x³, √x, 1/x, 1/x², 1/√x, ln x or log x. Error bars are propagated automatically (max–min method) and axis labels and units update.
- Rigorous max/min lines for linear fits, giving the gradient and intercept with uncertainties.
- Error bars on x and y. A note appears under the graph when they are too small to be seen at the plotted scale.
- Click a point to exclude it from the fit.
- Tangent tool for the gradient of a curve at any point.
- Residuals panel.
- Automatic or manual axis scaling, including negative ranges. Choice of gridlines, equation-box position and graph title.

**Analysis**
- Gradient and intercept quoted IB-style, e.g. `m = (4.0 ± 0.1) s² m⁻¹`, plus the percentage uncertainty.
- x- and y-intercepts, with uncertainties from the max/min lines. Interpolation: y at a given x, and x at a given y.
- Comparison with a literature value: percentage difference, and whether the accepted value lies within the experimental uncertainty.
- *Copy results as text* puts the analysis on the clipboard for a report.

**Saving and sharing**
- Work is autosaved in the browser (`localStorage`).
- *Save file* / *Open…* uses a `.graphlab.json` file.
- *Share link* puts the whole graph (data, settings, fit) into the URL itself, compressed. Nothing is sent to or stored on a server.
- Graph export as PNG (3× resolution) or SVG, or copied to the clipboard as an image.

**Help**
- An instruction sheet opens on start-up and is always available from **? Help**. Users can tick *Don't show this when the program starts*. This is remembered per browser, and unticking it in the Help sheet turns it back on.

---

## Deployment

The whole program is a single self-contained file, `index.html`, with no build step and no dependencies to install.

1. Put `index.html` (and this README) in a GitHub repository.
2. Connect the repository to Netlify. With no build command, set the publish directory to the repository root.
3. Netlify serves `index.html` at the site's root URL.

**Link previews (thumbnail on LinkedIn etc.).** Keep `og-image.png` next to `index.html`. In `index.html`, replace every `YOUR-SITE.netlify.app` with the real site address. Link previews need full addresses, not relative ones. After deploying, paste the link into LinkedIn's Post Inspector (linkedin.com/post-inspector) so LinkedIn fetches the new preview.

It also runs offline: open `index.html` directly in a browser. The only external request is for the IBM Plex fonts from Google Fonts, and the page falls back to system fonts without them. Exported graphs always use a standard sans-serif so they look the same on every computer.

**Browsers:** current Chrome, Edge, Firefox and Safari. *Copy image* needs a browser that allows images on the clipboard (Chrome, Edge, Safari). Elsewhere, use *Download PNG*.

---

## How the calculations work

**Fits.** Ordinary (unweighted) least squares on the points that are not excluded. Linear, proportional, quadratic and logarithmic fits are solved exactly. Power and exponential fits start from a log-linear estimate and are refined by Levenberg–Marquardt, so they minimise the error in the actual data rather than in its logarithm. R² = 1 − SS_res / SS_tot is computed on the plotted values.

**Max/min lines.** Each data point and its uncertainties form an error box. A straight line passes through a box when its value lies between the bottom and top of the box somewhere across the box's width. For a fixed sign of the gradient these conditions are linear in (m, c), so the set of lines that pass through *every* box is a convex polygon. The steepest and shallowest admissible lines are vertices of that polygon, and the program finds them exactly. Then Δm = (m_max − m_min) / 2 and Δc = |c_max − c_min| / 2. If no line passes through all the boxes, the program says so and falls back to the textbook method: lines through opposite corners of the first and last error bars.

**Propagation through linearisation.** For a transformed value f(v) with uncertainty Δv, the uncertainty is |f(v + Δv) − f(v − Δv)| / 2 (the max–min method).

**Rounding.** Uncertainties are quoted to 1 significant figure (2 is an option), and values are rounded to the same decimal place. Other results match the significant figures of the data (the most common number of significant figures in each column, from 2 to 6), or a fixed number chosen under *Axes & display*.

---

## Notes for teachers

- Strict precision can be switched off with the checkbox under the axis cards.
- The program starts empty. The *See example* checkbox at the top of the Data panel swaps in a pendulum dataset (T² against L, gradient = 4π²/g) for demonstrations. Unticking it restores the student's own graph untouched.
- *Clear all* (under the table) empties the title, axis cards and data, but keeps display settings.
- A share link is a convenient way for students to submit a graph: open it and you see exactly what the student did, including excluded points.
