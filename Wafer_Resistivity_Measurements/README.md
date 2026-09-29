# MOSAIC — Wafer Resistivity Measurements

This directory contains a streamlined analysis of the current–voltage (I–V)
measurements performed to characterise the doped-silicon wafers being considered
for MOSAIC.

The purpose of this analysis is deliberately focused: to extract reliable
resistance and bulk-resistivity information from the wafers for which our
bench-top contact geometry behaves well, while keeping the assumptions,
geometrical corrections, and limits of the measurement explicit and reproducible.

---

## 🧪 Data provenance and scope

The wafer-characterisation procedure was validated as part of the
LArCADe/MOSAIC programme using a device built by students many years ago as a Physics Olympiad project, with the probe geometry, operating procedure, and basic
consistency checks established before extending the study across the wider wafer
set.

The broader measurement campaign was carried out together with
**Simone Banaudi**, and includes repeated measurements of individual wafers,
resistance-stability studies, measurements over multiple current ranges, and
samples spanning a much wider range of manufacturer-specified resistivities.

The full dataset, together with Simone's independent analysis, is available here:

[**Simone Banaudi — LArCADe/MOSAIC wafer measurements**](https://github.com/Simone-Banaudi/LArCADe_MOSAIC_fermilab)

The present directory takes a deliberately narrower route through that dataset.
It focuses on the I–V measurements for which the contact behaviour is
sufficiently well understood to support a quantitative resistivity extraction,
and carries those measurements through a consistent geometry-corrected analysis.

A copy of the raw measurements required here is included locally so that the
workflow remains self-contained and reproducible.

### Why only a subset of the wafers?

The quantitative analysis presented here focuses on wafers with
manufacturer-specified bulk resistivities below approximately

```math
\rho \lesssim 0.01~\Omega\,\mathrm{cm}.
```

For these more heavily doped samples, the pogo-pin setup produced stable,
approximately linear I–V characteristics and measured resistances broadly
consistent with expectations from the manufacturer-provided resistivity ranges.

Measurements were also made on wafers with substantially higher nominal
resistivities, including samples at the level of
$1~\Omega\,\mathrm{cm}$ and above. In this regime, however, the behaviour changed
markedly. Apparent resistances frequently entered the
k$\Omega$–M$\Omega$ range, in some cases five to six orders of magnitude above
the resistance expected from the quoted bulk resistivity and wafer geometry.
Several of the corresponding I–V curves were also unstable or visibly
non-linear.

These values are genuine outputs of the measurement system, but they are **not**
interpreted here as measurements of the intrinsic bulk resistance of the silicon.

The most likely limitation is the electrical contact itself. The present setup
uses small-area, spring-loaded pogo pins rather than an industry-standard
semiconductor four-point-probe head with purpose-designed needle contacts. For
the more heavily doped wafers, the carrier concentration is sufficiently high
that the contact appears adequate and the wafer response dominates the
measurement. As the dopant concentration decreases and the resistivity rises,
contact resistance and metal–semiconductor interface effects can instead become
comparable to — or dominate over — the resistance that we are trying to measure.

Rather than ask the measurement to say more than it presently can, the
higher-resistivity wafers are therefore omitted from the quantitative results in
this streamlined analysis. Their measurements remain available in the wider
dataset and can be revisited once a more reliable contact scheme for lightly
doped silicon has been established.

---

# 📐 Four-point-probe geometry

The wafer measurements use four electrical contacts selected from six
spring-loaded pogo pins arranged approximately along a diameter of the wafer.

For each measurement, the two outer contacts source and sink current
(`FORCE HI` and `FORCE LO`), while the two inner contacts measure the voltage
difference (`SENSE HI` and `SENSE LO`).

The directly measured quantity is therefore

```math
R_{\mathrm{meas}}
=
\frac{|\Delta V|}{I}.
```

This is not, by itself, the sheet resistance. The conversion depends on both the
relative positions of the four contacts and the finite size of the wafer.

The derivation below is kept symbolic until the final step. This keeps the
treatment portable: if the probe positions, wafer dimensions, or measurement
configuration change, the same expressions can be reused without introducing a
new correction by hand.

---

## 1. General four-probe configuration

Consider a circular wafer of radius $R$, with four collinear contacts located at

```math
x_1 < x_2 < x_3 < x_4,
```

where the coordinates are measured relative to the centre of the wafer.

Current $I$ is sourced through $x_1$ and removed through $x_4$, while the
potential difference is measured between the inner contacts:

```math
\Delta V
=
V(x_3)-V(x_2).
```

For a wafer of thickness $t$ and bulk resistivity $\rho$, the sheet resistance is

```math
R_{\mathrm{s}}
=
\frac{\rho}{t}.
```

---

## 2. Infinite-sheet solution

For an infinitely extended conducting sheet, the potential along the probe axis
due to a point current source at $x_1$ and sink at $x_4$ is

```math
V_{\infty}(x)
=
\frac{I R_{\mathrm{s}}}{2\pi}
\ln\left|
\frac{x-x_4}{x-x_1}
\right|
+C.
```

The voltage measured by the two inner probes is therefore

```math
\Delta V_{\infty}
=
\frac{I R_{\mathrm{s}}}{2\pi}
\ln
\left|
\frac{
(x_3-x_4)(x_2-x_1)
}{
(x_3-x_1)(x_2-x_4)
}
\right|.
```

It is convenient to define the dimensionless geometry response

```math
H_{\infty}
=
\frac{1}{2\pi}
\left|
\ln
\left|
\frac{
(x_3-x_4)(x_2-x_1)
}{
(x_3-x_1)(x_2-x_4)
}
\right|
\right|.
```

Then

```math
\frac{|\Delta V|}{I}
=
R_{\mathrm{s}}H_{\infty},
```

and therefore

```math
\boxed{
R_{\mathrm{s}}
=
G_{\infty}\frac{|\Delta V|}{I}
}
```

with

```math
\boxed{
G_{\infty}
=
\frac{1}{H_{\infty}}.
}
```

The same geometry can also be described using the three consecutive probe
spacings

```math
a=x_2-x_1,
\qquad
b=x_3-x_2,
\qquad
c=x_4-x_3.
```

This gives

```math
\boxed{
G_{\infty}
=
\frac{2\pi}{
\ln\left[
\frac{(a+b)(b+c)}{ac}
\right]
}.
}
```

For the familiar case of equally spaced probes,

```math
a=b=c=s,
```

the expression reduces to

```math
\boxed{
G_{\infty}
=
\frac{\pi}{\ln 2}.
}
```

---

## 3. Finite circular wafer

Our wafers are not remotely infinite compared with the probe spacing. The wafer
edge therefore forms part of the measurement geometry rather than a distant
boundary that can safely be ignored.

The outer edge is treated as electrically insulating,

```math
\left.
\frac{\partial V}{\partial r}
\right|_{r=R}
=
0,
```

so that no current flows normally through the wafer boundary.

For source and sink contacts lying along a diameter, the finite-disk potential
along the same diameter can be written as

```math
V_{\mathrm{disk}}(x)
=
\frac{I R_{\mathrm{s}}}{2\pi}
\left[
\ln\left|
\frac{x-x_4}{x-x_1}
\right|
+
\ln\left|
\frac{1-xx_4/R^2}
     {1-xx_1/R^2}
\right|
\right]
+C.
```

Defining the dimensionless potential

```math
u(x)
\equiv
\frac{V_{\mathrm{disk}}(x)}
     {I R_{\mathrm{s}}},
```

gives

```math
u(x)
=
\frac{1}{2\pi}
\left[
\ln\left|
\frac{x-x_4}{x-x_1}
\right|
+
\ln\left|
\frac{1-xx_4/R^2}
     {1-xx_1/R^2}
\right|
\right].
```

The finite-wafer geometry response is therefore

```math
\boxed{
H_{\mathrm{disk}}
=
\left|
u(x_3)-u(x_2)
\right|
}
```

and the corresponding geometry factor is

```math
\boxed{
G_{\mathrm{disk}}
=
\frac{1}{H_{\mathrm{disk}}}.
}
```

The sheet resistance for an arbitrary four-probe configuration along the wafer
diameter is consequently

```math
\boxed{
R_{\mathrm{s}}
=
G_{\mathrm{disk}}
\frac{|\Delta V|}{I}.
}
```

The bulk resistivity follows directly:

```math
\boxed{
\rho
=
t\,G_{\mathrm{disk}}
\frac{|\Delta V|}{I}.
}
```

It is also useful to define the finite-size correction relative to the
infinite-sheet result,

```math
\boxed{
F
=
\frac{G_{\mathrm{disk}}}
     {G_{\infty}},
}
```

so that

```math
R_{\mathrm{s}}
=
F\,G_{\infty}
\frac{|\Delta V|}{I}.
```

This is the form used throughout the analysis: the probe coordinates and wafer
radius define the correction, rather than assigning a geometry factor
empirically.

---

# 🔢 Application to the present setup

The wafers used in this study have diameter

```math
D=100~\mathrm{mm},
```

or equivalently

```math
R=5~\mathrm{cm}.
```

The six nominal pogo-pin positions along the wafer diameter are

```math
(-4.5,\,-3,\,-1,\,+1,\,+3,\,+4.5)~\mathrm{cm}.
```

Three four-pin measurement configurations can therefore be formed:

| Probe set | Geometry |
| --- | --- |
| 1–4 | Outer |
| 2–5 | Centre |
| 3–6 | Outer |

The two outer configurations are mirror images of one another and therefore share
the same geometrical correction.

---

## Centre configuration

For the central four probes,

```math
(x_1,x_2,x_3,x_4)
=
(-3,-1,+1,+3)~\mathrm{cm},
```

so that

```math
a=b=c=2~\mathrm{cm}.
```

The infinite-sheet factor is therefore

```math
G_{\infty,\mathrm{centre}}
=
\frac{\pi}{\ln 2}
=
4.5324.
```

Evaluating the finite-disk expression gives

```math
H_{\mathrm{disk,centre}}
=
0.29740,
```

and hence

```math
G_{\mathrm{disk,centre}}
=
3.3625.
```

The corresponding finite-size correction is

```math
F_{\mathrm{centre}}
=
\frac{3.3625}{4.5324}
\simeq
0.742.
```

Thus, for the central configuration,

```math
\boxed{
R_{\mathrm{s,centre}}
=
3.3625
\frac{|\Delta V|}{I}.
}
```

---

## Outer configurations

For the left-hand outer configuration,

```math
(x_1,x_2,x_3,x_4)
=
(-4.5,-3,-1,+1)~\mathrm{cm},
```

which gives

```math
(a,b,c)
=
(1.5,2,2)~\mathrm{cm}.
```

The right-hand configuration,

```math
(x_1,x_2,x_3,x_4)
=
(-1,+1,+3,+4.5)~\mathrm{cm},
```

is its mirror image and therefore has the same geometry factor.

The infinite-sheet factor is

```math
G_{\infty,\mathrm{outer}}
=
\frac{2\pi}
{\ln(14/3)}
=
4.0788.
```

For the finite wafer,

```math
H_{\mathrm{disk,outer}}
=
0.34897,
```

giving

```math
G_{\mathrm{disk,outer}}
=
2.8656.
```

The finite-size correction is therefore

```math
F_{\mathrm{outer}}
=
\frac{2.8656}{4.0788}
\simeq
0.703.
```

Thus, for either outer configuration,

```math
\boxed{
R_{\mathrm{s,outer}}
=
2.8656
\frac{|\Delta V|}{I}.
}
```

For either geometry, the final conversion from sheet resistance to bulk
resistivity is simply

```math
\boxed{
\rho=tR_{\mathrm{s}}.
}
```

---

# 💻 Analysis and repository structure

The notebook in this directory fits the measured I–V response for each wafer and
probe configuration, extracts the corresponding voltage-to-current behaviour,
applies the appropriate finite-wafer geometry factor, and converts the resulting
sheet resistance into bulk resistivity using the wafer thickness.

The directory is organised as

```text
Wafer_Resistivity_Measurements/
├── README.md
├── wafer_resistance.ipynb
├── Data/
│   ├── wafers.csv
│   └── IV_wafer/
└── output/
```

`Data/IV_wafer/` contains the I–V scans used in this analysis, while
`Data/wafers.csv` contains the corresponding wafer metadata.

Derived fit results, summary tables, and other analysis products are written to
`output/`.

This directory is intentionally the compact, reproducible version of the
analysis. The wider measurement campaign — including repeat scans, stability
tests, and the higher-resistivity wafers currently excluded from the quantitative
interpretation — is preserved in the broader LArCADe/MOSAIC dataset available
through
[**Simone's repository**](https://github.com/Simone-Banaudi/LArCADe_MOSAIC_fermilab).

As the contact methodology improves, those measurements can be brought back into
the analysis rather than discarded; for now, the boundary of what we trust is
kept visible.