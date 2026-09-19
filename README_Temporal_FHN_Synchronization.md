# Temporal FitzHugh–Nagumo Synchronization with Binary Coupling and Emergency Rescue

This repository contains a Google Colab simulation of synchronization in a temporal directed network of identical FitzHugh–Nagumo systems.

The experiment tracks the network relative to a prescribed characteristic trajectory

\[
\dot s=f(s), \qquad s(0)=(0,0)^\top,
\]

and studies how a temporal network behaves when only two network-wide coupling strengths are available.

## Main idea

For every network vertex,

\[
W_i(t)=\frac12\|x_i(t)-s(t)\|_2^2,
\]

and the aggregate synchronization error is

\[
V(t)=\frac1n\sum_{i=1}^n W_i(t).
\]

The simulation uses two error levels,

\[
0<\varepsilon_{\mathrm L}<\varepsilon_{\mathrm H},
\]

where:

- \(\varepsilon_{\mathrm L}\) defines the desired low-error region;
- \(\varepsilon_{\mathrm H}\) is the upper tolerated error level.

At every temporal snapshot, the network can use either a low coupling strength \(w_{\mathrm L}\) or a high coupling strength \(w_{\mathrm H}\).

If emergency rescue is enabled and \(V(t)\) reaches \(\varepsilon_{\mathrm H}\), a graph- and state-dependent greedy procedure selects vertices for feedback pinning. Those vertices receive an input of the form

\[
u_i^{\mathrm{pin}}
=
-c_{\mathrm P}(x_i-s).
\]

The rescue input is removed after the aggregate error falls below \(\varepsilon_{\mathrm L}\).

## Temporal network

Every snapshot is a randomly generated directed graph whose underlying undirected graph is connected.

The construction does not impose:

- a permanent leader;
- a universal vertex;
- a DAG structure;
- a common topological hierarchy.

The detailed directed topology can therefore change from one snapshot to the next.

## Initially synchronized vertices

The user selects how many vertices begin exactly on the synchronous trajectory.

If \(r\) vertices are selected, then

\[
x_i(0)=s(0)
\]

for those \(r\) vertices.

They receive no special input after \(t=0\); they evolve under the same network equations as all other vertices.

## Interactive inputs

The notebook allows the user to select:

- number of vertices, from 2 to 100;
- number of temporal snapshots, from 2 to 100;
- number of initially synchronized vertices;
- initial-condition spread;
- low coupling strength \(w_{\mathrm L}\);
- high coupling strength \(w_{\mathrm H}\);
- whether emergency pinning is enabled;
- random seed.

The coupling strengths must satisfy

\[
0<w_{\mathrm L}<w_{\mathrm H}.
\]

## Output figures

The notebook generates two classes of figures.

### Aggregate synchronization error

The main figure plots

\[
V(t)
\]

against time.

It also displays:

- the lower level \(\varepsilon_{\mathrm L}\);
- the upper level \(\varepsilon_{\mathrm H}\);
- LOW/HIGH coupling decisions;
- pinning activation and release times.

The legend is placed outside the plotting area.

### Snapshot digraphs

Every temporal snapshot is saved as a separate graph image.

In the first snapshot, vertices that begin on the synchronous trajectory are indicated separately. During emergency rescue, vertices receiving pinning feedback are shown with square markers.

Snapshot files are named by snapshot number and action, for example:

```text
snapshot_001_LOW.eps
snapshot_006_HIGH_PINNING.eps
```

## Google Drive output

When run in Google Colab, the notebook mounts Google Drive and stores results under

```text
MyDrive/Temporal_FHN_Results/
```

Each simulation receives a run-specific folder containing:

```text
simulation_data.npz
run_summary.txt
aggregate_synchronization_error.eps
aggregate_synchronization_error.jpeg
snapshots/
```

The `snapshots/` directory contains one EPS and one JPEG image for every temporal snapshot.

EPS is the preferred format for thesis and publication figures because it preserves vector graphics. JPEG copies are also produced for quick viewing and sharing.

## Running the notebook

1. Open the notebook in Google Colab.
2. Run the setup and model cells.
3. Authorize Google Drive when prompted.
4. Choose the simulation parameters using the interactive controls.
5. Click **Run simulation**.
6. Run the **Aggregate synchronization-error figure** cell.
7. Run the **Snapshot digraph images** cell.
8. Use the **Snapshot viewer** cell to inspect any individual snapshot interactively.

## Requirements

The notebook uses:

- NumPy
- SciPy
- Matplotlib
- NetworkX
- ipywidgets

These packages are available in standard Google Colab environments.

## Files

The principal notebook is:

```text
Temporal_FHN_Threshold_Rescue_Colab_v5.ipynb
```

## Interpretation

The simulation is intended as a numerical illustration of synchronization control in a temporal directed network. The random snapshot sequence, binary coupling choices, aggregate error thresholds, and optional emergency pinning make it possible to observe both ordinary synchronization behavior and recovery when the prescribed upper error tolerance is reached.