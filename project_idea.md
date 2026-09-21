PROJECT TITLE

Automated MCB Short-Circuit Test Station with Hybrid Waveform Analysis

TEAM NAME

E-PASS


1. PROBLEM STATEMENT

MCB breaking-capacity testing as per IEC 60898-1:2015 requires controlled generation of severe short-circuit conditions with precise voltage, current, impedance, and power-factor values. The key problems identified in existing testing practices are:

• Manual Configuration & Inconsistency: Existing manual or semi-automated testing methods face challenges in precisely configuring the R (resistive) and XL (inductive) circuit combinations, increasing test time and affecting repeatability.
• High Safety Hazards: The generation of high fault currents, up to 10,000 A, introduces severe safety risks to personnel and test equipment when an operator is directly in the loop.
• Complex Multi-Rating Support: The system must support diverse MCB configurations (SP, SPN, DP, TP, FP) with ratings from 0.5 A to 63 A, making manual re-configuration complex and error-prone.
• Waveform Analysis & Traceability Gap: Fast transient inception and sub-cycle interruption details are difficult to capture manually, and maintaining complete digital records requires an integrated automated testing platform.

The core objective is to develop an automated system that precisely controls test parameters, generates required fault conditions safely, captures waveforms at high speed, and provides repeatable and traceable test results while eliminating manual operator exposure.



2. PROPOSED SOLUTION

We propose an Automated MCB Short-Circuit Test Station which automates the complete testing workflow from test-condition configuration and controlled fault generation to waveform acquisition, analysis and digital reporting.

The system uses an isolated and current-limited AC source, selectable R-XL impedance banks and a controlled fault-making contactor to establish the required test condition. The MCB under test is mounted in a universal test fixture.

A WCS1700 Hall-effect current sensor module is used for current waveform acquisition and an isolated voltage sensing circuit is used to capture the corresponding voltage waveform.

The STM32F401RE Nucleo-64 is used for high-speed data acquisition and waveform processing, while the PLC handles machine control, test sequencing and safety-related functions.

The HMI provides the interface for entering test parameters, monitoring the test and viewing the final results.


3. SYSTEM WORKFLOW

The overall system follows this sequence:

User enters the required test parameters through the HMI.

The system calculates the required impedance based on the target voltage, current and power factor.

Z = V / I

R = Z × PF

XL = Z × sqrt(1 - PF²)

The calculated R and XL values are matched with the available R-XL bank steps.

The required resistor and inductor combinations are selected.

The PLC checks the required safety and test conditions.

After the test conditions are satisfied, the controlled fault-making contactor initiates the fault condition.

The voltage and current sensing system captures the electrical response.

The WCS1700 captures the current waveform and the voltage sensing circuit captures the voltage waveform.

The STM32F401RE acquires the signals through ADC and DMA.

The complete waveform is stored and processed.

The waveform is passed through the AHTA analysis pipeline.

Direct electrical features and AHTA waveform features are then combined through the proposed fusion logic.

The MCB fault response is evaluated.

The measured results are compared with the defined test requirements and applicable IEC 60898-1:2015 requirements.

The system generates the PASS/FAIL result.

The configuration, measurements, waveform, AHTA results and final result are stored as a digital test record.

The stored records can be viewed on a daily or weekly basis, including total MCBs tested, passed and failed and individual test details.


4. ELECTRICAL SYSTEM

The prototype uses a low-voltage, isolated and current-limited electrical setup for safe validation.

Main electrical sections include:

15 A isolated AC transformer

Selectable R-XL impedance bank

MCB test fixture

WCS1700 Hall-effect current sensor

Isolated voltage sensing circuit

Schneider Electric LC1D18B7 contactor for controlled fault initiation

PLC for machine control and safety

STM32F401RE Nucleo-64 for waveform acquisition and AHTA processing

HMI for test parameter entry and result display

Appropriate protection, interlocks and emergency stop arrangements


5. R-XL AND POWER FACTOR CONTROL

The R-XL network is used to control the electrical conditions of the test circuit.

The system calculates the required impedance from the target voltage, current and power factor.

Z = V / I

R = Z × PF

XL = Z × sqrt(1 - PF²)

The calculated values are then matched with the available resistor and inductor combinations.

The control system selects the corresponding R and XL steps to achieve the required test condition as closely as possible.

This allows different test conditions to be created without manually changing the circuit for every test.


6. FAULT GENERATION

The controlled fault circuit uses a contactor to initiate the fault condition after the required test parameters and safety conditions are satisfied.

The Schneider Electric LC1D18B7 contactor is used as the controlled switching element in the prototype fault circuit.

The prototype is intentionally current-limited and isolated so that the complete control, sensing and waveform-analysis methodology can be validated safely before considering higher-current industrial implementation.


7. WAVEFORM ACQUISITION

The current waveform is captured using the WCS1700 Hall-effect current sensor module.

The voltage waveform is captured using an isolated voltage sensing circuit.

The sensor outputs are provided to the STM32F401RE ADC through suitable signal conditioning and protection.

ADC-DMA is used so that waveform samples can be transferred to memory continuously without requiring the CPU to handle every individual sample.

This allows the STM32 to capture the complete waveform during the fault event and process it using the AHTA logic.


8. AHTA – ADAPTIVE HYBRID TRANSIENT ANALYSIS

AHTA is our proposed waveform-analysis logic developed specifically for analysing the complete MCB fault waveform.

The purpose of AHTA is not to replace direct electrical calculations. Direct electrical features such as RMS, peak current, I²t, dI/dt and trip time are calculated directly from the measured waveform.

AHTA provides additional information about the waveform behaviour and combines periodic and transient analysis.

The proposed AHTA flow is:

Captured Waveform
        ↓
Noise Filtering
        ↓
Fourier Series Analysis
        ↓
Inverse Fourier Series Reconstruction
        ↓
Discrete Wavelet Transform
        ↓
Waveform and Transient Features
        ↓
Feature Fusion
        ↓
MCB Performance Evaluation


9. FFS – FOURIER SERIES

The fault waveform contains a periodic AC component.

Fourier Series is used to represent the periodic waveform using its fundamental and harmonic components.

This allows the system to study the periodic and harmonic characteristics of the measured waveform.


10. IFFS – INVERSE FOURIER SERIES

The selected Fourier components can be reconstructed using Inverse Fourier Series.

The reconstructed waveform provides a reference representation of the periodic part of the measured signal.

The measured waveform and reconstructed periodic waveform can then be compared to identify changes that are not explained by the normal periodic component.


11. DWT – DISCRETE WAVELET TRANSFORM

DWT is used to analyse sudden and time-localised changes in the waveform.

This is important because MCB fault behaviour contains events such as fault initiation, rapid current rise and interruption.

DWT helps identify these short-duration changes and provides additional information about the transient behaviour of the waveform.


12. FILTERING

A Butterworth low-pass filter is used as part of the signal-processing stage to reduce unwanted high-frequency noise while preserving important waveform characteristics.

The filter parameters will be selected based on the actual sampling rate and frequency characteristics of the waveform so that important fault information is not removed.


13. DIRECT ELECTRICAL FEATURES

The following parameters are calculated directly from the acquired waveform:

Peak Current (Ip)

RMS Current

I²t

dI/dt

Trip/Interruption Time

Power Factor

These parameters provide the direct electrical measurements required for evaluating the MCB response.


14. FEATURE FUSION

The direct electrical features and AHTA waveform characteristics are not treated as separate final decisions.

The proposed fusion stage combines both sources of information.

Direct electrical measurements describe values such as current magnitude, energy and trip time, while AHTA provides information about periodic behaviour, waveform reconstruction and transient events.

The combined information is used for MCB fault-response and interruption evaluation.


15. MCB PERFORMANCE EVALUATION

The system evaluates the measured MCB response using the required electrical parameters and waveform characteristics.

The evaluation considers parameters such as:

Peak current

RMS current

I²t

dI/dt

Power factor

Trip/interruption time

Periodic waveform characteristics

Transient characteristics

The measured results are compared with the applicable test requirements and defined limits.

The final system produces a PASS/FAIL result based on the complete evaluation.


16. MATLAB / SIMULINK DEVELOPMENT

MATLAB/Simulink is used during the development stage for electrical modelling and algorithm validation.

The electrical model is used to study the R-XL combinations, current, voltage and power factor.

The waveform-analysis algorithms are first tested using simulated and measured signals.

The AHTA processing flow is developed and validated in MATLAB before being implemented in embedded software.

The development flow is:

MATLAB/Simulink
        ↓
Electrical Model
        ↓
Waveform Analysis
        ↓
AHTA Validation
        ↓
Embedded C
        ↓
STM32F401RE


17. EMBEDDED IMPLEMENTATION

After validation in MATLAB/Simulink, the required algorithms are implemented using Embedded C.

The STM32F401RE Nucleo-64 performs real-time waveform acquisition and processing.

The actual testing system does not depend on MATLAB for operation.

MATLAB is used for development and validation, while the STM32 performs the required acquisition and embedded processing during the actual test.


18. PLC AND STM32 ARCHITECTURE

The PLC and STM32 are used for different purposes.

The PLC is responsible for machine control, test sequencing and safety-related functions.

The STM32F401RE is responsible for high-speed waveform acquisition and AHTA processing.

This avoids using an expensive high-speed PLC for waveform processing while still retaining PLC-based industrial control and safety.

The architecture therefore provides a balance between industrial machine control and low-cost high-speed embedded processing.


19. HMI

The HMI allows the operator to enter the required test parameters and monitor the test process.

The HMI can display:

MCB type

MCB rated current

Target test current

Voltage

Power factor

R-XL configuration

Live waveform

Calculated electrical parameters

AHTA results

PASS/FAIL status

Test information

The HMI acts as the main user interface for operating the automated test station.


20. DIGITAL TEST REPORT

The system generates a digital report after each test.

The report contains:

Test configuration

MCB details

R-XL settings

Voltage and current measurements

Captured waveform

AHTA results

Calculated electrical parameters

PASS/FAIL result

Date and time

The test records are stored with timestamps so that previous tests can be reviewed.


21. DAILY AND WEEKLY TEST HISTORY

The stored test records can be used to monitor testing activity.

The user can view:

Total MCBs tested today

Number of PASS results

Number of FAIL results

Individual test details

Test timestamps

Weekly total tests

Weekly PASS count

Weekly FAIL count

This provides a clear testing history and improves digital traceability of the MCB testing process.


22. WHY OUR APPROACH

Our approach combines automated electrical test control with high-speed waveform analysis.

A high-speed PLC system capable of waveform acquisition and advanced waveform processing can increase system cost.

Instead, the PLC is used for machine control and safety, while the STM32F401RE is used for high-speed acquisition and AHTA processing.

This division allows us to maintain industrial-style control while using a lower-cost embedded platform for waveform analysis.


23. FEASIBILITY

PLC + STM32 Architecture:
PLC handles machine control and safety, while STM32 handles high-speed data acquisition and waveform analysis.

Embedded Real-Time Processing:
The STM32F401RE supports fast waveform sampling, AHTA analysis and real-time test control.

Smart Test Data Management:
The system stores test results and shows how many MCBs were tested, passed and failed on a daily or weekly basis.

Scalable Implementation:
The current low-current prototype can be scaled to higher fault currents for industrial testing.


24. VIABILITY

Reduced Testing Effort:
Automated R-XL selection and test sequencing can reduce manual configuration and operator dependency.

Repeatable Testing:
Programmable test parameters provide consistent test conditions across multiple MCB tests.

Digital Traceability:
Automatic storage of configuration, waveforms, calculated parameters and PASS/FAIL results provides complete test history.

Industrial Scalability:
PLC-based machine control and modular power circuitry allow migration towards higher-current industrial systems.

Research-to-Industry Path:
MATLAB/Simulink → Embedded C → STM32 provides a structured path from algorithm development to deployable firmware.


25. EXISTING SOLUTION LIMITATIONS

Manual Configuration:
Selection of R, XL and test conditions can require significant manual setup and adjustment.

High-Energy Testing:
High short-circuit currents create major safety requirements for personnel, equipment and test infrastructure.

Advanced Waveform Analysis Gap:
Detailed waveform and transient analysis may be handled as a separate measurement or diagnostic step rather than being integrated directly into the automated test workflow.

Traceability:
Maintaining complete digital records of configuration, waveforms and test history can be improved through an integrated digital testing platform.


26. STRATEGIES FOR OVERCOMING CHALLENGES

Current and Energy Limitation:
Use an isolated and current-limited prototype source with suitable protection.

Fail-Safe Control:
Use PLC/safety interlocks, emergency stop, enclosure interlock and fault-trigger permissives.

R-XL Selection:
Characterize the actual R and XL elements and use measured values for automatic combination selection.

High-Speed Acquisition:
Use synchronized ADC and DMA sampling on STM32 with suitable isolated sensing and signal conditioning.

Algorithm Validation:
Validate FFS, IFFS and DWT processing against simulated and measured waveforms in MATLAB/Simulink before embedded deployment.

Progressive Scaling:
Validate the complete control and analysis methodology at low energy first, then scale the power stage for industrial implementation.


27. IMPACTS AND BENEFITS

Faster Testing:
Automated test setup and fault initiation reduce manual intervention and support faster testing of multiple MCBs.

Better Test Accuracy:
Controlled R-XL and power-factor settings provide more consistent test conditions.

Deeper Fault Analysis:
AHTA provides additional information about periodic and transient waveform behaviour along with direct electrical measurements.

Easy Test Tracking:
Automatic storage provides daily and weekly information about total tests, PASS results and FAIL results.

Safer Operation:
PLC-based machine control and safety interlocks reduce operator involvement during controlled fault generation.

Lower System Cost:
Using the PLC for machine control and the STM32F401RE for high-speed waveform processing avoids the need for an expensive high-speed PLC architecture.


28. CASE STUDY – INDUSTRY FEASIBILITY DISCUSSION

As part of developing our MCB short-circuit testing system, we discussed our proposed concept with an electrical industry professional to understand how the approach could be applied in a practical testing environment.

We presented our overall testing concept, including controlled fault generation, R-XL selection, waveform acquisition and MCB performance evaluation.

The main purpose was to get practical feedback on whether our proposed AHTA waveform analysis and electrical feature fusion could be useful for analysing MCB fault behaviour.


29. CONCEPT PRESENTED DURING CASE STUDY

We explained our proposed AHTA (Adaptive Hybrid Transient Analysis) approach and how it fits into the complete MCB testing system.

The proposed method combines direct electrical measurements such as RMS current, peak current, I²t, dI/dt, trip time and power factor with waveform characteristics obtained through FFS, IFFS and DWT.

We also explained that AHTA is not intended to replace basic electrical calculations, but to combine waveform analysis with electrical measurements for a more complete evaluation of MCB behaviour.


30. KEY INSIGHTS FROM THE DISCUSSION

The discussion helped us understand the importance of accurate electrical measurements, controlled test conditions and reliable waveform acquisition.

We also discussed the need for proper signal acquisition and filtering so that unwanted noise does not affect the extracted waveform features.

The discussion helped us refine the role of AHTA as a waveform-analysis layer working together with direct electrical measurements.


31. AHTA TESTING IN ELECTRICAL LABORATORY

We tested our proposed AHTA control and analysis logic in the college electrical laboratory using a real AC sine-wave signal.

The objective was to check whether our waveform-processing approach could capture and analyse an actual electrical waveform before applying it to MCB fault-waveform analysis.

The test allowed us to work with a real electrical signal instead of relying only on simulated data.


32. EXPERIMENTAL SETUP

A real AC sine-wave signal was used as the input.

The waveform was captured through the measurement and acquisition setup and processed using our MATLAB-based AHTA analysis.

The experiment included an AC sine-wave source, measurement setup, data acquisition system and computer-based waveform processing.

The captured waveform was then analysed using the proposed processing flow.


33. AHTA VALIDATION PROCESS

The captured waveform was first processed to reduce unwanted noise and improve the signal quality.

The waveform was then analysed using FFS, IFFS and DWT.

FFS was used to study the periodic and harmonic components.

IFFS was used to reconstruct the selected periodic component.

DWT was used to observe changes and transient characteristics in the waveform.

The results were observed together to understand how the different stages contribute to the overall waveform analysis.


34. PARAMETERS OBSERVED DURING TESTING

RMS value

Peak value

Frequency

Waveform characteristics

Harmonic components

Transient changes

Reconstructed waveform

These observations helped us understand how the different AHTA stages behave when applied to a real AC waveform.


35. VALIDATION OUTCOME

The real AC waveform was successfully captured and processed through the developed analysis flow.

The FFS/IFFS stages provided information about the periodic waveform, while DWT helped identify changes in the signal at specific time intervals.

The test also helped us observe the effect of signal filtering and understand how noise can influence waveform analysis.

This provided an initial practical validation of the AHTA concept on a real electrical waveform.


36. FUTURE DEVELOPMENT

The next stage is to improve the AHTA logic using different waveform conditions and fault-like transient signals.

We will study the selection and tuning of the FFS, IFFS and DWT processing methods in more detail and discuss the approach with electrical and signal-processing experts.

The fusion logic will also be developed further so that direct electrical features and AHTA waveform characteristics can work together for MCB performance evaluation.

After further validation, the algorithms will be deployed on the STM32F401RE and integrated with the complete automated MCB test station.


37. COMPLETE SYSTEM FLOW

HMI
↓
Test Parameter Selection
↓
R-XL and Power Factor Calculation
↓
Automatic R-XL Selection
↓
PLC Safety and Test Sequence
↓
Controlled Fault Generation
↓
MCB Under Test
↓
Voltage + Current Waveform Acquisition
↓
STM32F401RE ADC + DMA
↓
Noise Filtering
↓
AHTA
↓
FFS + IFFS + DWT
↓
AHTA Waveform Features
+
Direct Electrical Features
↓
Feature Fusion
↓
MCB Performance Evaluation
↓
IEC 60898-1:2015 Based Evaluation
↓
PASS / FAIL
↓
Digital Test Report
↓
Daily / Weekly Test History


38. EXPECTED OUTCOME

The final system is intended to provide an automated and repeatable platform for MCB short-circuit testing.

It will automate test-condition selection, controlled fault generation, waveform acquisition and result reporting.

The AHTA logic will provide additional waveform information by combining periodic and transient analysis with direct electrical measurements.

The system will generate traceable digital test records and provide daily and weekly testing statistics.

The current prototype is intentionally isolated and current-limited to validate the complete control, acquisition and analysis architecture before scaling the power stage towards higher-current industrial implementation.