# TLV76701 simulation review

Reviewed from the supplied PSpice project archives on 2026-09-15. This is a file and saved-result review; the simulations were not rerun here. Original circuit, model and result bytes are preserved. Backup files, lock/cache files and duplicate startup screenshots in the load-transient case were omitted.

## Verified saved settings

| Setting | Startup | Load transient |
| --- | --- | --- |
| Input | 0 to 5 V at 200 us; 1 us rise/fall | Same |
| Input pulse width / period | 4 ms / 2 s | 10 ms / 2 s |
| Run time | 3 ms | 5 ms |
| Maximum timestep | 20 ns | 100 ns |
| Save data after | 0 | 0 |
| Skip initial bias point | Off | Off |
| Resistor load | 33 ohm (about 100 mA) | 330 ohm (about 10 mA) |
| Additional current sink | None | 0 to 90 mA; delay 2 ms; rise/fall 1 us; width 1 ms; period 10 ms |

Both generated netlists use the TLV76701_TRANS subcircuit with EN tied to VIN, a 100 kohm upper / 32 kohm lower feedback divider, 1 uF + 100 nF input capacitance, 2.2 uF + 100 nF output capacitance and 10 pF OUT-to-FB capacitance. The capacitor values and connections match the supplied schematic screenshot. The total external load is approximately 10 to 100 mA; plot I(RLOAD)+I(I1). I(RLOAD) alone remains near 10 mA.

Both saved output logs report JOB CONCLUDED, with no simulation error or convergence warning found. The model signature logs report Signature valid. The ordinary generated-file overwrite notice is not a simulation failure. All included PNGs decode successfully, including load_transient_schematic.png.

## Results recorded with the supplied cursor screenshots

| Quantity | Approximate result | Interpretation |
| --- | --- | --- |
| Startup rise time | 0.42 ms | Manually selected points near 10% and 90% of nominal 3.3 V; excludes startup delay |
| Load application dip | 10.6 mV | Relative to the output immediately before the roughly 10-to-100 mA step |
| Load removal overshoot | 11.0 mV | Relative to the output immediately before the roughly 100-to-10 mA step |

These are rounded cursor-based simulation readings, not newly computed global extrema or hardware measurements. The load returns near 3 ms (pulse fall starts at 3.001 ms). Recovery time was not quantified.

## Opening and reproducing

1. Extract the entire archive on a Windows computer with PSpice for TI, keeping each case folder intact.
2. Open TLV76701_TRANS.opj inside the desired case folder and select STARTUP-trans.
3. Check the saved settings against the table and run. Use V(VIN), V(OUT), I(RLOAD), and for the load-transient case I(RLOAD)+I(I1).
4. Saved waveform data and plot settings are under TLV76701_TRANS-PSpiceFiles/STARTUP/trans/.

The OPJ design, profile and local-model references are relative. The generated circuit references ../../../tlv76701_trans.lib. Standard nom_pspti.lib and nom.lib depend on the local PSpice installation. Historical absolute paths remain in GUI state and provenance comments; reopening on another computer has not been tested. Open PAGE1 through the project tree if a restored window points to the old location.

The schematic displays a legacy TPS74601P_TRANS label, but the actual generated netlist instantiates TLV76701_TRANS and the supplied model declares that subcircuit. Do not rename model symbols merely to change the displayed label.

## Scope and model attribution

This is nominal schematic-level startup and load-transient evidence. It does not establish PCB thermal performance, continuous current capability, production tolerances, noise, or full stability margins. Capacitor ESR, DC-bias derating and PCB parasitics are not represented by the ideal external components in these netlists.

The supplied encrypted model is Texas Instruments TLV76701P transient model, Final 1.00, dated 21DEC2018. Its original copyright and notices are retained. The header states noise and temperature effects are not modelled; it says quiescent/shutdown currents are modelled, whereas the example schematic note says quiescent current is not modelled. No quiescent-current conclusion is made here. Third-party model files remain subject to their original terms; this package assigns them no new license.
