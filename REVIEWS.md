SUBMISSION: 276
TITLE: On the Predictive Power of Compute Utilization Metrics for GPU Power Modeling

----------------------- REVIEW 1 ---------------------

SUBMISSION: 276
TITLE: On the Predictive Power of Compute Utilization Metrics for GPU Power Modeling

----------- Overall evaluation -----------
SCORE: 1 (weak accept)
----------- Paper summary -----------
This paper performs a nice analysis to relate GPU utilisation, the model flops approach, and actual power measurements in terms of predictive capabilities in AI workloads, in particular LLM training. This is a very timely topic, obviously. I doubt that this has never been done before, as the authors claim (the business is just too big), but I agree with the authors that not too many studies have been published on the topic yet: My guess that from a vendor perspective in hyperscaling, this information is just too convidential and mission-critical to consider making it public.
----------- Strengths -----------
* important, timely topic
* well written, clear outset
* results fully reproducible, all data and software on github
* thorough experimental evaluation
* all findings fully substantiated
----------- Weaknesses -----------
* the potential target audience is probably not the largest
* the "gaming GPUs" are most probably not the number one target platform for these kinds of studies...
* there are small (but many) deficiencies in the methodology and in the interpretation of the results, e.g., related to the variance of certain measurements
* overall, the paper is a very good effort, but maybe not quite there yet
----------- Comments for authors -----------
This is a nice study that has been carried out quite well. If the paper does not get accepted for Europar, I recommend to improve on the methodology, maybe try to get access to proper GPUs, and resubmit elsewhere, or next year again at Europar.


----------------------- REVIEW 2 ---------------------

SUBMISSION: 276
TITLE: On the Predictive Power of Compute Utilization Metrics for GPU Power Modeling

----------- Overall evaluation -----------
SCORE: -1 (weak reject)
----------- Paper summary -----------
The paper explores the usage of simple and easy access utilization metrics of GPUs' monitoring as prediction proxy for power modeling.
----------- Strengths -----------
The explored space is wide and spans multiple parameters.
----------- Weaknesses -----------
There are structural deficiencies in the paper. The paper is poorly written with no clear story being presented. The sections are not particularly coherent and the ideas presented and analyzed are not made clear to the reader.  The terminology used is mixed and confusing. Overall I believe significant rewriting is in order.

The relatively extensive real-hardware vs telemetry analysis does not link to the MFU vs GPU-Utilization power prediction.

The results of Table 2 advocating MFU over Utilization have low base values with high variance, rendering them not trustworthy.
----------- Comments for authors -----------
Thani you for submitting your paper in EuroPar. My overall assessment of this paper is that both the methodology and results presented are not clearly detailed and explained. The authors should clearly mention what models and inputs they are using and how they set up their experiments. They should also go into more detail regarding the results presented in Table 2 explaining the varying number of configurations per parameter as well as the reason behind the modest explanatory power of the studied predictors. Following are some more detailed comments:

Mentioned independent variables “model architecture” and “warmup and cooldown phases” are not detailed. What models/values are being evaluated?
What is the “statistically significant convergence criterion”?
The telemetry vs external power comparison is quite extensive despite it being relatively irrelevant to the paper’s story. It is also incomplete since it lacks details regarding the tools used for external power measurement as well as explainability regarding the offset between external meter power and GPU-reported power.
The results presented in Table 2 are not clear. Does each per-parameter configuration correspond to a combination of the rest of the independent variables? If so, why is the number of configurations varied within each parameter? Why is AMD MI210 included in this analysis when it comes to GPU Utilization since its results would just skew the overall results due to its limitation (constant 1 measurement)? How does this model achieve the highest R^2 despite it being essentially random due to the aforementioned limitation?
Much of the terminology used throughout the paper is not explained. e.g. “compute-bound regions”.
In Figure 3 why is the dtype the only studied variable, especially since it is mentioned that batch size also significantly affects the results?
“the main exception is RTX 4070 Ti under float32 (R2 = 0.5223), likely due to runtime noise”: Why is the error concentrated at 100% utilization?
Certain citations are missing:
CalFlops
Hydra
“Analytical and statistical models may therefore be used to estimate power from hardware activity counters and performance metrics”: AccelWattch


----------------------- REVIEW 3 ---------------------

SUBMISSION: 276
TITLE: On the Predictive Power of Compute Utilization Metrics for GPU Power Modeling

----------- Overall evaluation -----------
SCORE: -1 (weak reject)
----------- Paper summary -----------
The paper investigates the effectiveness of different metrics in predicting GPU power consumption for AI workloads, specifically focusing on large language model (LLM) training. This paper explores whether Model FLOPS Utilization serve as a reliable alternative. The main output of this work is the development of a benchmark toolkit for comparing various metrics.
----------- Strengths -----------
The topic covered in the paper is important for many AI factories that spent a large amount of energy.
----------- Weaknesses -----------
The authors claim that in GitHub they publicly share scripts, results, and analysis. However, after inspection, I found that this repository only contains scripts for preparing plots and uncategorized csv with data.

As the benchmark is proposed as one of the main outputs of the work, this benchmark should be shared.

The paper does not detail the configuration of the benchmark, as it is stated to have 504 workloads.
----------- Comments for authors -----------
As stated before, it is recommended to share the benchmark with the community as 504 configurations is a massive amount of testbeds. However the paper does not describe the range and the scope

From the methodology point of view, the paper lacks of rigurousness. The benchmark is not described at all. The paper does not describe the metrics employed and the hardware used in terms of the GPUs hosts.

One of the negative aspects of the paper is the limitation of NVIDIA GPUs. Most of the hardware evaluated lacked external power validation, and the study was predominantly focused on NVIDIA architectures. In case of AMD accelerators, the work did not reach a conclusion.

Finally, in all plots, the authors employ colors for different series but the paper does not describe the match.

In the first equation, the authors does not explain that is really is FLOPSmax and FLOPSreq and in which way this metrics are obtained.


----------------------- REVIEW 4 ---------------------

SUBMISSION: 276
TITLE: On the Predictive Power of Compute Utilization Metrics for GPU Power Modeling

----------- Overall evaluation -----------
SCORE: 1 (weak accept)
----------- Paper summary -----------
This paper discusses two metrics for utilization of GPUs and their predictive power for modeling GPU power consumption, with the goal of informing future modeling and simulation efforts for architectures and datacenters.
----------- Strengths -----------
* clearly written, with enough details in the metrics and methods used
* extensive data and figures to support the takeaways
----------- Weaknesses -----------
* limited explanatory power of these experiments
* limited audience for such study at Europar
----------- Comments for authors -----------
This is well though-out study of an interesting topic area: how to predict the power consumption of GPU kernels based on the very limited set of metrics that can be monitored?

This paper is clearly written, with plenty of detailed explanations of the experimental setup and of the concerns of the study. My main concern with this paper is the lack of deep exploration of when the two metrics under study behave differently. In particular, MFU being a pure FLOPS metric, one would have expected experiments that would target specifically cases where the GPU memory hierarchy is the main bottleneck, as a way of differentiating with GPU utilization.

As a result, while this paper explores plenty of interesting experiments, the reader is left with little information on how arithmetic intensity, or memory requirements, of a kernel, can impact this predictive power. Attempting to identify cases where neither metrics is a good predictor would also have been very valuable.