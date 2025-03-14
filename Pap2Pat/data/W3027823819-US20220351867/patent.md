# DESCRIPTION

## FIELD OF THE DISCLOSURE

The present disclosure concerns, but is not limited to, a computer-implemented method and a system for dating blood pools lying on a substrate.

## TECHNICAL BACKGROUND

Despite the recent development of forensic science, estimating bloodstain age has not yet been convincingly implemented in routine crime scene investigation. Current methods for estimating blood pools age rely on complex procedures requiring specialist knowledge and/or sophisticated, expensive machines, Spectroscopy-based techniques and hyperspectral imaging are currently the techniques closest to implementation due to their widespread availability. However, both types of techniques suffer from low accuracy, are destructive and non-easily transportable on every crime scene.

The article by Laan et al., Morphology of drying blood pools, Forensic Sci. Int. 267, pages 104-109, 2016, attempts to characterize the drying process of blood pools and identifies different drying stages. However, this attempt represents only a very preliminary step towards the establishment of a predictive drying model ideally aimed at leading to an accurate determination of the time elapsed between a given time at which a blood pool is considered and a time at which the blood pool has been spread, Indeed, the various characteristics of a blood pool, such as for example the size and shape thereof, together with the variability of the environmental conditions, lead overall to a daunting challenge in establishing such drying model, since no apparent correlation exists that could weight all cited blood pool characteristics and environmental conditions.

The article by Choi et al., Highly sensitive and accurate estimation of bloodstain age using smartphone, Biosens. Bioelectron. 130, pages 414-419, 2019, describes a system for estimating the age of bloodstains that combines three analyses, namely blood pool analysis, crack ratio analysis and colorimetric analysis, which are all based on the image analysis of a bloodstain. Parameters are extracted from such image analysis and the extracted parameters are then compared to corresponding parameters of reference images stored in a database.

Another method for estimating the age of bloodstains is described in the article by Thanakiatkrai et al., Age estimation of bloodstains using smartphones and digital image analysis, Forensic Sci. Int. 233, pages 288-297, 2013. Such method is based on colorimetry and plots color parameters extracted from digital images of bloodstains against time, and then compares the results to a calibration curve. The effects of further parameters on the estimation, such as the type of smartphone camera, person-to-person variation, temperature, humidity, light exposure, anticoagulant, and substrate, are further discussed in the article.

All prior art methods merely provide rough age estimations, and lack a drying model reliably representing a blood pool's drying process. There thus still exists a need for a method offering a high accuracy and reliability in dating blood pools, while remaining simple to be implemented, not destructive or altering, and transferable on the field.

## SUMMARY OF THE DISCLOSURE

In what follows, the term “comprise” is synonym of (means the same as) “include” and “contains”, is inclusive and open and does not exclude other non-recited elements: Moreover, in the present disclosure, the terms “about” and “substantially” are synonyms of (mean the same as) a margin less and/or more than to 10% of the respective value.

One of the objectives of the present disclosure is to provide a method for dating blood pools lying on a substrate.

According to a first aspect of the present disclosure, such an Objective is attained by a computer implemented method for dating a blood pool lying on a substrate, comprising:


- - determining, based on at least one picture of a blood pool lying on
    a substrate taken at a given time t, the drying front of the blood
    pool at the given time; and
  - determining, based on the determined drying front of the blood pool
    at the given time t and on a drying model, the time elapsed between
    the given time t and an initial time t₀ at which the blood pool
    initiated a drying process on the substrate,  
    wherein determining the drying front of the blood pool at the given
    time t comprises determining the wet perimeter P_(t) of the blood
    pool at the given time t, and  
    wherein the drying model correlates the time elapsed with a blood's
    diffusion coefficient D_(blood) measured for a plurality of
    predetermined environmental conditions and a plurality of initial
    blood pool conditions.

In the present description and in the claims, by “blood pool”, it is meant any blood configuration (FIG. 1B) intermediate between a drop (FIG. 1A), which has a curved surface, and an infinite stretch of liquid having a flat surface (FIG. 1C), as illustrated by FIG. 1. According to one or more embodiments, a blood pool presents a substantially flat, limited surface.

The method of the disclosure may be applied to any blood pool drying on a substrate, independently from the volume thereof. However, according to one or more embodiments, the method is particularly adapted to blood pools having a volume greater than 170 μL. Indeed, when the blood pool has a volume smaller than 170 for substrates giving a contact angle of about 45° and for a surface tension at the gas-liquid interface of 60 mPa·s, the blood configuration tends to take the shape of a drop.

The method according to the present disclosure can for example be successfully implemented in routine crime scene investigation, by providing a real-time, accurate estimation of the time elapsed after a victim's death, i.e. the postmortem interval (PIM), and/or the approximate time at which blood was spread on a substrate and thus at which a crime involving bleeding was committed.

Hence, by determining, based on a drying front of the blood pool, which is simply determined on the basis of one or more pictures, and based on a drying model, the method according to the disclosure provides a real-time and accurate estimation of the time at which the blood of a victim, and/or a culprit, was spilled.

By drying process, it is meant a process which comprises at least one evaporation process. The drying process initiated by the blood pool comprises the evaporation of water contained within the blood of the blood pool. Such drying process may be function of the environmental conditions to which the blood pool is exposed.

According to one or more embodiments, the drying model correlates the time elapsed with a blood's diffusion coefficient Dblood by means of blood pool parameters including the blood pool's height h.

According to one or more embodiments, the time elapsed between the given time t and the initial time t0 is determined by:

providing a database comprising sets of predetermined environmental and initial blood pool conditions including initial areas A0, the sets of predetermined environmental and initial blood pool conditions being associated with corresponding sets of measured blood pool parameters including blood's diffusion coefficients Dblood and blood pool's heights h.

determining the environmental conditions to which the blood pool is exposed at the given time t, and an initial area A0 of the blood pool corresponding to the area of the blood pool at the initial time t0,

comparing the determined environmental conditions and the determined initial area A0 with the sets of predetermined environmental and initial blood pool conditions, and

determining a set of measured blood pool parameters comprising the blood's diffusion coefficient Dblood, and the blood pool's height h associated with the set of predetermined environmental and initial blood pool conditions matching with the determined environmental conditions and the determined initial area A0.

According to one or more embodiments, the measured blood pool parameters including blood's diffusion coefficients Dblood and blood pool's heights h included in the sets associated with the corresponding sets of predetermined environmental and initial blood pool conditions comprised in the database, are measured at the predetermined environmental and initial blood pool conditions of the corresponding associated set.

According to one or more embodiments, the predetermined environmental conditions comprise at least the temperature and humidity to which the blood pool is exposed, and the nature of the substrate on which the substrate lies.

According to one or more embodiments, determining the time elapsed between the given time and a time at which the blood pool initiated a drying process on the substrate is done by calculation of such time based on a predetermined mathematical equation representing a predetermined drying model.

According to one or more embodiments, the drying model comprises a function correlating the time elapsed between the given time t and the initial time t0 with the blood's diffusion coefficient Dblood, the blood's diffusion coefficient Dblood having a substantially constant value during the time elapsed between the given time t and the initial time t0 for the same environmental conditions.

According to one or more embodiments, the blood's diffusion coefficient is expressed according to the following equation:

Dblood=KiLkL*0.5  (1)

wherein:


- - D_(blood) is the blood's diffusion coefficient;
  - K_(i) is a coefficient of transfer from the liquid state into the
    gaseous state and is expressed according to the following equation:

\(\begin{matrix}
{K_{i} = {J^{*}\frac{RT}{MP_{w}}}} & (2)
\end{matrix}\)


- - - wherein:
      - M is the molecular weight of water;
      - P_(w) is the saturation vapor pressure of water at the surface
        of the blood pool;
      - R is the ideal gas constant;
      - T is the temperature of the environment to which the blood pool
        is exposed; and
      - J\* is the evaporation rate of the blood pool and is expressed
        according to the following equation:

\(\begin{matrix}
{J^{*} = {\frac{\delta m}{\delta t} \times \frac{1}{A_{0}}}} & (3)
\end{matrix}\)


- - - - - wherein:
        -  δm/δt is the mass variation of the blood pool with time; and
        - ′A₀ is the area of the blood pool at a time t₀ at which the
          blood pool initiated a drying process on the substrate;

  - L_(k) is the Knudsen layer and is expressed according to the
    following equation:

\(\begin{matrix}
{L_{k} = \frac{k_{B}T}{\pi d^{2}P_{a}}} & (4)
\end{matrix}\)


- - - wherein:
      - k_(B) is Boltzmann's constant;
      - d is the molecular diameter of water molecules; and
      - P_(a) is the atmospheric pressure;

  - L\* is a shape factor and is expressed according to the following
    equation:

\(\begin{matrix}
{L^{*} = \frac{A_{0}}{P_{t}h}} & (5)
\end{matrix}\)


- - - wherein:
      - h is the blood pool's height; and
      - P_(t) is the wet perimeter.

According to one or more embodiments, the blood's diffusion coefficient Dblood may be measured experimentally. According to one or more embodiments, Dblood may be measured for a plurality of predetermined environmental conditions or, for example, for a set of predetermined environmental conditions susceptible of representing the environmental conditions to which a blood pool may be commonly exposed.

According to one or more embodiments, the environmental conditions comprise at least the temperature and humidity to which the blood pool is exposed, and the nature of the substrate on which the substrate lies. The temperature and humidity may refer to the temperature and humidity in the direct surroundings of the blood pool, for example within a 3 m radius from the blood pool. The nature of the substrate may comprise the composition of the substrate.

Determining the drying front of the blood pool at the given time t comprises determining the wet perimeter of the blood pool at the given time t. Such wet perimeter of the blood pool at the given time t is designated by reference sign Pt in FIG. 2 which illustrates a drying blood pool. At a given time t, the wet perimeter is the path surrounding the wet area of the blood pool. The wet area of the blood pool at the given time t is designated by reference sign At in FIG. 2 and is delimited by a wet perimeter Pt at time t. The wet area At corresponds to the remaining blood pool's liquid surface at time t, by opposition to the blood pool's dry surface at time t. By analogy, the wet area of the blood pool at time t0 at which the blood pool initiated a drying process on the substrate is designated by reference sign A0 in FIG. 2 and is delimited by a wet perimeter P0 at time t0. The area. A0 is the maximum spreading area of the blood pool on the substrate.

According to one or more embodiments, determining the drying front of the blood pool at the given time t further comprises determining the wet area of the blood pool at the given time t, i.e., At.

According to one or more embodiments, the method comprises taking at least one picture of the blood pool at a given time t, for example a plurality of pictures of the blood pool starting from the given time t. According to one or more embodiments, the method comprises taking one picture per 10 minutes for a predetermined period of time, such as for example for 1 hour. In this manner, the margin of error in dating the blood pool may thereby be lowered.

According to one or more embodiments, such at least one picture of the blood pool can be taken in several ways. For example, depending on the country, the forensic standards for taking a picture on a crime scene may vary, and the method according to the invention may be adapted accordingly. According to one or more embodiments, the at least one picture is taken on top of the blood pool, at a distance from the blood pool comprised between 0.1 in and 3 m, depending on the size of the blood pool. According to one or more embodiments, the at least one picture is taken on top of the pool, as perpendicularly as possible from the pool with a ruler beside.

According to one or more embodiments, the at least one picture may be taken with a conventional digital camera. A conventional digital camera may for example be integrated in a smartphone or a tablet.

According to one or more embodiments, the resolution of the at least one picture is more than 1000 pixels2, for example more than 2000 pixels2.

According to one or more embodiments, the determining of the drying front of the blood pool is performed via image processing of the at least one picture. For example, when an image processing software such as imageJ or for example a Python code is used, a minimum resolution of 2000 pixels2 may be advantageous in order to better focus on the blood pool only and an overexposure and/or a flash may result in a better contrast between the wet substrate and the dry substrate.

In the article Laan et al., Morphology of drying blood pools, Forensic Sci. Int. 267, pages 104-109, 2016, the authors have shown that blood pools' drying process can be separated into five main different drying stages: coagulation, gelation, rim desiccation, center desiccation, and final desiccation. According to one or more embodiments of the present disclosure, the step of taking at least one picture of the blood pool at a given time is performed either during the rim desiccation stage or during the center desiccation stage of the blood pool's drying process and, when the method comprises taking a plurality of pictures of the blood pool at a given time, in at least one of these stages. Contrary to the first drying stage, i.e., coagulation, wherein the wet area at a given time t is equal or very close to the wet area at time t0, the wet area at a given time t during the rim desiccation or center desiccation stages is sufficiently different from the wet area at time t0. Likewise, in these embodiments, the wet perimeter at time t is sufficiently different from the wet perimeter at time t0. Also, in these embodiments, the wet area and the wet perimeter at a given time t are not equal to 0, while this is the case in the last drying stage, i.e., in the final desiccation.

Thus, according to these embodiments, if the blood pool is in a coagulation stage, the at least one picture should be taken in a later stage, for example starting from the rim desiccation stage. If the blood pool is in a still later drying stage such as the final desiccation stage, the least one picture may be taken, but the wet perimeter and/or the wet area of the blood pool may not be determined anymore, since they are both equal to zero in such stage. However, in the majority of cases, the blood pools found on crime scenes have not yet reached this final stage of the blood pool's drying process.

According to one or more embodiments, the drying model comprises a function correlating the mass variation of the blood pool with time with the wet area of the blood pool at the given time t. According to one or more embodiments, such function comprises correlation coefficients which may be measured experimentally, for a plurality of predetermined environmental conditions or, for example, for a set of predetermined environmental conditions susceptible of representing the environmental conditions to which a blood pool may be commonly exposed.

According to one or more embodiments, the method further comprises determining the blood pool's height. According to one or more embodiments, determining the blood pool's height comprises the use of a statistical value of the blood pool's height. The statistical value may for example be an average value of blood pool's height measured for the same environmental conditions. According to one or more embodiments, a statistical value of the blood pool's height may be the average value of blood pool's height measured for a predetermined number of blood pools having the same area of the blood pool at a time at which the blood pool initiated a drying process on the substrate, and for the same environmental conditions. According to one or more embodiments, the predetermined number of blood pools is no less than 5, for example no less than 10, and may less than 20, and their mass may be comprised, for example, from 0.1 g to 50 g.

According to one or more embodiments, the function correlating the mass variation of the blood pool with time with the wet area of the blood pool at the given time t is expressed according to the following equation:

\(\begin{matrix}
{\frac{\delta m}{\delta t} = {- \frac{\alpha{m_{0}\left\lbrack {1 - \left( \frac{A_{t}}{A_{0}} \right)} \right\rbrack}^{\beta}}{\delta t}}} & (6)
\end{matrix}\)

wherein:


- - α and β are correlation coefficients constant at the environmental
    conditions to which the blood pool is exposed at the given time t;
  - A_(t) is the wet area of the blood pool at the given time t;
  - δt is the time elapsed between the given time t and the initial time
    t₀;
  - δm is the mass variation of the blood pool during the time elapsed
    between the given time t and the initial time t₀;
  - m₀ is the mass of the blood pool at the initial time t₀ and is
    expressed as according to the following equation:

m0=A0ρh  (7)

wherein:


- - ρ is the blood's volume weight.

According to one or more embodiments, the function correlating the mass variation of the blood pool with time with the wet area of the blood pool at the given time may be expressed according to the following equation:

\(\begin{matrix}
{\frac{m_{t}}{m_{0}} = {1 - {\alpha\left\lbrack {1 - \frac{A_{t}}{A_{0}}} \right\rbrack}^{\beta}}} & \left( 6^{\prime} \right)
\end{matrix}\)

wherein mt is the mass variation of the blood pool at the given time t.

This equation represents the variation of the normalized mass mt/m0 as a function of the normalized wet area (At/A0), and involves correlation coefficients α and β.

As mentioned above, according to one or more embodiments, the function correlating the mass variation of the blood pool with time with the wet area of the blood pool at the given time t comprises correlation coefficients α and β which may be measured experimentally, for a plurality of predetermined environmental conditions or, for example, for a set of predetermined environmental conditions susceptible of representing the environmental conditions to which a blood pool may be commonly exposed.

According to one or more embodiments, the sets of measured blood pool parameters including blood's diffusion coefficients Dblood and blood pool's heights h, and which are associated with the sets of predetermined environmental and initial blood pool conditions comprised in the database as defined above with respect to one or more embodiments of the method of the first aspect, further comprise the correlation coefficients α and β.

According to one or more embodiments, the correlation coefficients α and β may be measured for a predetermined number of blood pools having different masses. For example, the correlation coefficients α and β may be determined out of a pool of at least 4 blood pools having different masses, for example a first blood pool weighing from 1 g to 2 g, a second blood pool weighing from 5 g to 6 g, a third blood pool weighing from 10 g to 12 g, and a fourth blood pool weighing from 30 g to 35 g. According to one or more embodiments, a fitting curve may be deduced from the superimposition of the predetermined number of corresponding variations of the normalized mass mt/m0 as functions of the normalized wet area At/A0. Such fitting curve may thus provide the correlation coefficients α and β.

According to one or more embodiments, the time elapsed δt between the given time t and the initial time t0 is expressed according to the following equation:

\(\begin{matrix}
{{\delta t} = \frac{\alpha{Rk}_{B}T^{2}A_{0}^{1/2}h^{1/2}{\rho\left\lbrack {1 - \left( \frac{A_{t}}{A_{0}} \right)} \right\rbrack}^{\beta}}{\pi d^{2}MP_{t}^{1/2}D_{blood}P_{w}P_{a}}} & (8)
\end{matrix}\)

The equations (1) and (6) are combined with each other into equation (8) by considering that the evaporation rate J* may be expressed as the variation of mass with time per surface unit according to equation (3).

Hence, as apparent from equation (8), the inventors have surprisingly managed to devise one equation correlating all drying parameters of the drying model according to one or more embodiments with the elapsed time δt. The inventors have demonstrated that despite the complexity of establishing a drying model taking account of the variability of all parameters at stake during the blood pools' drying process, a reliable and simple drying model exists and may be used to develop a corresponding method for dating blood pools, in a fast, reliable, and accurate manner. According to one or more embodiments, the drying model may take account of the blood pool's size and/or shape, which are reflected by mathematical equations used in the modeling. For example, when one considers equation (8), correlation coefficients α and β may take into account the size of the blood pool. The dimensionless factor L* takes account of the shape of the blood pool. Indeed, L*, which is directly related to the drying front, for example according to equation (5), correlates the shape properties of the blood pool with the wet perimeter.

According to one or more embodiments, the blood's volume weight is substantially comprised between 1,040 g/mL and 1.066 g/mL.

According to one or more embodiments, the method comprises a step of determining the atmospheric pressure: According to one or more embodiments, the atmospheric pressure is substantially equal to 1.01325·105 Pa.

According to one or more embodiments, the saturation vapor pressure of water at the surface of the blood pool is deduced from determination of the temperature and humidity to which the blood pool is exposed at the given time t.

According to one or more embodiments, the molecular diameter of water molecules is substantially equal to 0.343 nm.

According to one or more embodiments, determining the environmental conditions to which a blood pool is exposed may be done by measuring the environmental conditions, directly and/or indirectly, on the crime scene. According to one or more embodiments, requesting the measurement of environmental conditions may for example be envisaged. For example, the method may comprise sending the GPS location of the blood pool to a weather forecast virtual platform and obtaining the measured data sent back by the weather forecast virtual platform. Such an indirect measurement may be beneficial when the blood pool has been drying outdoors, since environmental parameters such as the temperature and humidity may sensibly vary with time. A liability level of the recorded values can then be assessed, depending on the available data.

Indoors, in contrast, the environment of the blood pool is expected to be stable in the majority of cases, and a standard ambient temperature, for example of 20° C., may be used. Alternatively, a direct measuring of the environmental parameters may be performed.

According to one or more embodiments, the substrate on which the blood pool lies may be porous. According to one or more embodiments, the substrate may be non-porous, i.e., so that the blood may remain substantially stagnant prior to drying. According to one or more embodiments, the substrate on which the blood pool lies may be selected in the group comprising tiling, linoleum, varnished parquet, unvarnished parquet, glass, paper, wood (for example oak), fabric, asphalt, carpet, grass.

According to one or more embodiments, the blood comprised in the blood pool has a hematocrit level Ht comprised between 35% and 55%, for example between 40% and 50%. The hematocrit level Ht is comprised between 40.7% and 50.3% for men and between 36.1% and 44.3% for women. The inventors have shown that the hematocrit level does not have a major influence on the blood pool's drying process. However, according to one or more embodiments, the method may be specifically adapted to men or women. According to one or more embodiments, the method comprises the use of two databases, one male database specifically adapted to men and one female database specifically adapted to women. For example, the method according to one or more embodiments may include a step of recording the sex of the blood giver. This will orient the method towards the use of a corresponding male or female database.

Hence, according to one or more embodiments of the method, the method envisages the determination of certain parameters, such as for example Pt, At and/or A0, on the basis of the image processing of at least one blood pool's picture, and the determination of parameters related to the environment of the blood pool, such as temperature, humidity, and to the nature of the substrate. All other parameters may be collected and saved beforehand in a database. Such a dating method allows a very precise and reliable dating of the blood pool. The inventors have indeed realized that such a method is particularly adapted to the dating of blood pools and shows excellent results in dating blood pools. For example, an error margin of less than 4% could be obtained in dating blood pools that had been drying for more than 8 h, by using the method according to one or more embodiments of the present disclosure. Such method for dating blood pools according to the invention provides a low-cost, rapid, easy-to-use and truly portable alternative to more complicated analysis using specialized and highly expensive equipment, e.g. spectroscopy and/or HPLC.

According to one or more embodiments, the method according to the invention comprises the repetition of at least the following steps a predetermined number of times:

determining, based on at least one picture of a blood pool lying on a substrate taken at a given time t, the drying front of the blood pool at the given time t; and

determining, based on a drying model and the determined drying front of the blood pool at the given time (t), the time elapsed between the given time t and a time t0 at which the blood pool initiated a drying process on the substrate.

Such repetition may for example occur at predetermined time intervals during a predetermined period.

In this manner, an average value of the elapsed time between the given time t and a time t0 at which the blood pool initiated a drying process on the substrate may be obtained.

According to a second aspect, the present disclosure is directed to a system for dating a blood pool lying on a substrate, comprising:

means for determining, based on at least one picture of a blood pool lying on a substrate taken at a given time t, the drying front of the blood pool at the given time t; and

means for determining, based on a drying model and on the determined drying front of the blood pool at the given time t, the time elapsed between the given time t and an initial time t0 at which the blood pool initiated a drying process on the substrate,

wherein the means for determining the drying front of the blood pool at the given time t determines the wet perimeter Pt of the blood pool at the given time t; and

wherein the means for determining the time elapsed between the given time t and the initial time t0 correlates the time elapsed with a blood's diffusion coefficient Dblood measured for a plurality of predetermined environmental conditions and a plurality of initial blood pool conditions.

According to one or more embodiments, the means for determining the drying front of the blood pool at the given time t comprises at least one image processing software that performs an image processing of the at least one picture of the blood pool. Such at least one image processing software may for example be installed on a smartphone or tablet. According to one or more embodiments, the at least one image processing software is selected in the group comprising ImageJ, a Matlab code, a Python code.

According to one or more embodiments, the means for determining the time elapsed between the given time t and the initial time t0 comprises a calculation software.

According to one or more embodiments, the system further includes a database comprising sets of predetermined environmental and initial blood pool conditions including initial areas (A0), the sets of predetermined environmental and initial blood pool conditions being associated with corresponding sets of measured blood pool parameters including blood's diffusion coefficients (Dblood) and blood pool's heights (h).

According to one or more embodiments, the system according to the second aspect may be used to implement the method according to the first aspect. Such system may be a mobile system offering an easy-to-use and portable means for dating blood pools, wherein no training is necessary, such as for example a smartphone wherein a dating software is installed that performs the steps of the method for dating blood pools. Such dating software may provide an accurate age estimate by taking account of a user's inputs, such as pictures of the blood pool or determined environmental and initial blood pool conditions impacting the drying of blood pools.

According to one or more embodiments, the system further comprises a camera for taking the at least one picture of the blood pool lying on a substrate. According to one or more embodiments, the system comprises a conventional digital camera. Such conventional digital camera may for example be integrated in a smartphone or a tablet. The model of the camera may have an influence on, for example, the color rendering of the picture of the blood pool. Hence, according to one or more embodiments, measures may be taken so that parameters of the smartphone are set so that they may be easily adapted to different forensic standards, depending on the geographical zone of a crime scene.

According to one or more embodiments, the system further comprises a thermometer and a hygrometer for determining the temperature and, respectively, the humidity to which the blood pool is exposed.

According to one aspect, the present disclosure concerns a method for dating a blood pool lying on a substrate, comprising:


- - determining, based on at least one picture of a blood pool lying on
    a substrate taken at a given time, the drying front of the blood
    pool at the given time; and
  - determining, based on the determined drying front of the blood pool
    at the given time and a drying model, the time elapsed between the
    given time and a time at which the blood pool initiated a drying
    process on the substrate,
  - wherein the drying model comprises a function correlating the time
    elapsed between the given time (t) and a time (t₀) at which the
    blood pool initiated a drying process on the substrate with a
    blood's diffusion coefficient, the blood's diffusion coefficient
    having a substantially constant value during the time elapsed
    between the given time (t) and a time (t₀) at which the blood pool
    initiated a drying process on the substrate for the same
    environmental conditions,
  - wherein the blood's diffusion coefficient is expressed according to
    the following equation:

Dblood=KiLkL*0.5  (1)


- - wherein:
    - D_(blood) is the blood's diffusion coefficient;
    - K_(i) is a coefficient of transfer from the liquid state into the
      gaseous state and is expressed according to the following
      equation:

\(\begin{matrix}
{K_{i} = {J^{*}\frac{RT}{MP_{w}}}} & (2)
\end{matrix}\)


- - - wherein:
      - M is the molecular weight of water;
      - P_(w) is the saturation vapor pressure of water at the surface
        of the blood pool;
      - R is the ideal gas constant;
      - T is the temperature of the environment to which the blood pool
        is exposed; and
      - is the evaporation rate of the blood pool and is expressed
        according to the following equation:

\(\begin{matrix}
{J^{*} = {\frac{\delta m}{\delta t} \times \frac{1}{A_{0}}}} & (3)
\end{matrix}\)


- - - - - wherein:
        -  δm/δt is the mass variation of the blood pool with time; and
        -  A₀ is the area of the blood pool at a time (t₀) at which the
          blood pool initiated a drying process on the substrate;

    - L_(k) is the Knudsen layer and is expressed according to the
      following equation:

\(\begin{matrix}
{L_{k} = \frac{k_{B}T}{{\pi d}^{2}P_{a}}} & (4)
\end{matrix}\)


- - - - wherein:
        - k_(B) is Boltzmann's constant:
        - d is the molecular diameter of water molecules; and
        - P_(a) is the atmospheric pressure;

    - L\* is a shape factor and is expressed according to the following
      equation:

\(\begin{matrix}
{L^{*} = \frac{A_{0}}{P_{t}h}} & (5)
\end{matrix}\)


- - - - wherein:
        - h is the blood pool's height; and
        - P_(t) is the wet perimeter.

According to one or more embodiments, determining the drying front of the blood pool at the given time t comprises determining the wet perimeter of the blood pool at the given time t. According to one or more embodiments of the method according to the embodiments described at page 15, line 14, determining the drying front of the blood pool at the given time t further comprises determining the wet area of the blood pool at the given time t, i.e., At. According to one or more embodiments, the drying model comprises a function correlating the mass variation of the blood pool with time with the wet area of the blood pool at the given time t. According to one or more embodiments, the method further comprises determining the area of the blood pool at a time at which the blood pool initiated a drying process on the substrate and the environmental conditions to which the blood pool is exposed at the given time.

According to one or more embodiments of the method according to the embodiments described at page 15, line 19 and according to the embodiments described at page 15, line 21 when they also include the features of the embodiments described at page 15, line 19, the function correlating the mass variation of the blood pool with time with the wet area of the blood pool at the given time (t) is expressed according to the following equation:

\(\begin{matrix}
{\frac{\delta m}{\delta t} = {- \frac{\alpha{m_{0}\left\lbrack {1 - \left( \frac{A_{t}}{A_{0}} \right)} \right\rbrack}^{\beta}}{\delta t}}} & (6)
\end{matrix}\)


- - wherein:
    - α and β are correlation coefficients constant at the environmental
      conditions to which the blood pool is exposed at the given time t;
    - A_(t) is the wet area of the blood pool at the given time t;
    - δt is the time elapsed between the given time t and a time t₀ at
      which the blood pool initiated a drying process on the substrate;
    - δm is the mass variation of the blood pool during the time elapsed
      between the given time t and a time t₀ at which the blood pool
      initiated a drying process on the substrate;
    - m₀ is the mass of the blood pool at a time t₀ at which the blood
      pool initiated a drying process on the substrate and is expressed
      as according to the following equation:

m0=A0ρh  (7)


- - wherein:
    - ρ is the blood's volume weight.

According to one or more embodiments of the method according to the embodiments described at page 15, line 24, the time elapsed δt between the given time t and the time t0 at which the blood pool initiated a drying process on the substrate is expressed according to the following equation:

\(\begin{matrix}
{{\delta t} = \frac{\alpha{Rk}_{B}T^{2}{A_{0}}^{1/2}h^{1/2}{\rho\left\lbrack {1 - \left( \frac{A_{t}}{A_{0}} \right)} \right\rbrack}^{\beta}}{{\pi d}^{2}{{MP}_{t}}^{1/2}D_{blood}P_{w}P_{a}}} & (8)
\end{matrix}\)

According to one or more embodiments of the method according to the embodiments described at page 15, line 24 or according to the embodiments described at page 16, line 15, the method further comprises providing a database comprising sets of measured blood pool parameters at different conditions, wherein each set of measured blood pool parameters comprises the blood's diffusion coefficient, the blood pool's height, and the correlation coefficients, wherein the different conditions comprise predetermined environmental and initial blood pool conditions impacting the drying of blood pools, According to one or more embodiments of the method according to the embodiments described at page 16, line 20, the predetermined environmental conditions impacting the drying of blood pools comprise at least the temperature and humidity of the environment to which the blood pool is exposed, and the nature of the substrate on which the blood pool lies. According to one or more embodiments of the method according to the embodiments described at page 16, line 20 or according to the embodiments described at page 16, line 26, the predetermined initial blood pool conditions impacting the drying of blood pools comprise the area of the blood pool at a time at which the blood pool initiated a drying process on the substrate. According to one or more embodiments of the method according to the embodiments described at page 16, line 30 when they include the features of the embodiments described at page 16, line 20 when they include the features of the embodiments described at page 15, line 24, when they include the features of the embodiments described at page 15, line 21, the method providing the database further comprises the steps of comparing the determined environmental conditions to which the blood pool is exposed at the given time t and the determined area of the blood pool at a time t0 at which the blood pool initiated a drying process on the substrate, with the sets of predetermined environmental and initial blood pool conditions impacting the drying of blood pools comprised in the database.

According to one or more embodiments, the method comprises the repetition of at least the following steps a predetermined number of times: determining, based on at least one picture of a blood pool lying on a substrate taken at a given time t, the drying front of the blood pool at the given time t; and determining, based on a drying model and the determined drying front of the blood pool at the given time (t), the time elapsed between the given time t and a time t0 at which the blood pool initiated a drying process on the substrate. Such repetition may for example occur at predetermined time intervals during a predetermined period. In this manner, an average value of the elapsed time between the given time t and a time t0 at which the blood pool initiated a drying process on the substrate may be obtained.

According to one aspect, the present disclosure is directed to a system for dating a blood pool lying on a substrate, comprising:

means for determining, based on at least one picture of a blood pool lying on a substrate taken at a given time t, the drying front of the blood pool at the given time t; and

means for determining, based on a drying model and the determined drying front of the blood pool at the given time t, the time elapsed between the given time t and a time t0 at which the blood pool initiated a drying process on the substrate,


- - wherein the drying model comprises a function correlating the time
    elapsed between the given time (t) and a time (t₀) at which the
    blood pool initiated a drying process on the substrate with a
    blood's diffusion coefficient, the blood's diffusion coefficient
    having a substantially constant value during the time elapsed
    between the given time (t) and a time (t₀) at which the blood pool
    initiated a drying process on the substrate for the same
    environmental conditions,
  - wherein the blood's diffusion coefficient is expressed according to
    the following equation:

Dblood=KiLkL*0.5  (1)


- - wherein:
    - D_(blood) is the blood's diffusion coefficient;
    - K_(i) is a coefficient of transfer from the liquid state into the
      gaseous state and is expressed according to the following
      equation:

\(\begin{matrix}
{K_{i} = {J^{*}\frac{RT}{MP_{w}}}} & (2)
\end{matrix}\)


- - - wherein:
      - M is the molecular weight of water;
      - P_(w) is the saturation vapor pressure of water at the surface
        of the blood pool;
      - R is the ideal gas constant;
      - T is the temperature of the environment to which the blood pool
        is exposed; and
      - J\* is the evaporation rate of the blood pool and is expressed
        according to the following equation:

\(\begin{matrix}
{J^{*} = {\frac{\delta m}{\delta t} \times \frac{1}{A_{0}}}} & (3)
\end{matrix}\)


- - - - - wherein:
        -  δm/δt is the mass variation of the blood pool with time; and
        -  A₀ is the area of the blood pool at a time (t₀) at which the
          blood pool initiated a drying process on the substrate;

    - L_(k) is the Knudsen layer and is expressed according to the
      following equation:

\(\begin{matrix}
{L_{k} = \frac{k_{B}T}{{\pi d}^{2}P_{a}}} & (4)
\end{matrix}\)


- - - wherein:
      - k_(B) is Boltzmann's constant;
      - d is the molecular diameter of water molecules; and
      - P_(a) is the atmospheric pressure;
    - L\* is a shape factor and is expressed according to the following
      equation:

\(\begin{matrix}
{L^{*} = \frac{A_{0}}{P_{t}h}} & (5)
\end{matrix}\)


- - - wherein:
      - h is the blood pool's height; and
      - P_(t) is the wet perimeter.

According to one or more embodiments of the system according to the aspect described at page 17, line 22, the means for determining, based on at least one picture of a blood pool lying on a substrate taken at a given time t, the drying front of the blood pool at the given time t, comprises at least one image processing software that performs an image processing of the at least one picture of the blood pool. Such at least one image processing software may for example be installed on a smartphone or tablet. According to one or more embodiments, the at least one image processing software is selected in the group comprising ImageJ, a Matlab code, a Python code. According to one or more embodiments, the means for determining, based on a drying model and the determined drying front of the blood pool at the given time t, the time elapsed between the given time t and a time t0 at which the blood pool initiated a drying process on the substrate comprises a calculation software. According to one or more embodiments, the system further comprises a database comprising sets of measured blood pool parameters at different conditions, wherein each set of measured blood pool parameters comprises a blood's diffusion coefficient, a blood pool's height, and parameters correlating the mass variation of the blood pool with time with the wet area of the blood pool at the given time t, and wherein the different conditions comprise predetermined environmental and initial blood pool conditions impacting the drying of blood pools. According to one or more embodiments, the system further comprises a camera for taking the at least one picture of the blood pool lying on a substrate. According to one or more embodiments, the system comprises a conventional digital camera. According to one or more embodiments, the system further comprises a thermometer and a hygrometer for determining the temperature and, respectively, the humidity to which the blood pool is exposed.

The embodiments described above are not exhaustive. In particular, it is understood that additional embodiments can be considered on the basis of different combinations of the explicitly described embodiments. Unless otherwise specified in the present disclosure, it will be apparent to the skilled person that all the embodiments described above can be combined together. For example, unless otherwise specified, all features of the embodiments described above, whether they refer to the method or the system for dating blood pools, can be combined with or replaced by other features from other embodiments.

## DETAILED DESCRIPTION OF EMBODIMENTS

Embodiments of the present disclosure will now be described in detail with reference to the accompanying figures. In the following detailed description of embodiments of the present disclosure, numerous specific details are set forth in order to provide a more thorough understanding of the present disclosure. However, it will be apparent to one of ordinary skill in the art that the present disclosure may be practiced without these specific details. In other instances, well-known features have not been described in detail to avoid unnecessarily complicating the description.

The following description provides a non-limiting example of a system for dating a blood pool lying on a substrate, according to one or more embodiment of the present disclosure.

The following description also provides non-limiting examples of a method for dating a blood pool lying on a substrate according to one or more embodiments of the present disclosure.

Example of a System for Dating a Blood Pool Lying on a Substrate

An example of system for dating a blood pool lying on a substrate is shown in FIG. 3. The system comprises a smart phone 1 comprising a conventional camera (not shown) for taking pictures and an image processing software for determining, based on at least one picture 2 of a blood pool 3 lying on a substrate 4 taken at a given time t, the drying front 5 of the blood pool 3 at the given time t. The system further comprises a calculation software for determining, based on the determined drying front of the blood pool at the given time t and a drying model, the time elapsed between the given time t and a time t0 at which the blood pool initiated a drying process on the substrate.

In the present example, the image processing software is image). However, other image processing softwares may be run in the smart phone. Also, according to other embodiments, the image processing software may be run in other devices.

The image processing software may be integrated within a dating software.

The imageJ software may be used to determine the area of the blood pool at a time t0 at which the blood pool initiated a drying process on the substrate from a picture of the blood pool. The type of image may first be changed from RGB color type to 8 or 16-bit type image, the black and white threshold may then be manually adjusted in order to define the contours of the blood pool. To measure the area of the blood pool, an “analyze particles” function may used. The minimum pixel size may be set to 2000 pixels2 in order to focus on the blood pool only. Holes may be included in order to exclude reflects that may be present on the picture, and the outlines of the measured area may be displayed with the results in order to verify the accuracy of the area's measurement.

The calculation software may be integrated within a dating software, which may be the same as the above-mentioned dating software comprising the above-mentioned image processing software.

In the present example, the dating software is configured to request the measurement of two environmental conditions, namely temperature and humidity, to a weather forecast virtual platform and to obtain the measurement by sending the GPS location to the weather forecast virtual platform.

In the present example, the dating software is also configured to ask a user to upload the at least one picture taken by the camera and to fill out blank fields corresponding to a third environmental condition, namely the nature of the substrate.

In this example of system, the system further comprises a database including sets of measured blood pool parameters at different conditions. Each set of measured blood pool parameters comprises a blood's diffusion coefficient Dblood, a blood pool's height h, and parameters correlating the mass variation of the blood pool with time with the wet area of the blood pool at the given time t. The different conditions comprise predetermined environmental and initial blood pool conditions impacting the drying of blood pools. The predetermined environmental conditions comprise the temperature and humidity to which the blood pool is exposed and the nature of the substrate on which the substrate lies. The predetermined initial blood pool conditions comprise the area of the blood pool at a time t0 at which the blood pool initiated a drying process on the substrate.

As mentioned above, this example of system for dating blood pool according to the embodiments of the disclosure comprises a smartphone including a conventional camera, which can be used to take one or more pictures of a blood pool lying on a substrate. However, further configurations of the system may be envisaged, in which the system also comprises, for example, a thermometer and/or a hygrometer.

Example of a Method for Dating a Blood Pool Lying on a Substrate

The following describes an example of a computer-implemented method for dating a blood pool lying on a substrate according to embodiments of the present disclosure.

The method for dating a blood pool lying on a substrate comprises determining, based on at least one picture of a blood pool lying on a substrate taken at a given time t, the drying front of the blood pool at the given time t, and determining, based on the determined drying front of the blood pool at the given time t and on a drying model, the time elapsed between the given time t and an initial time t0 at which the blood pool initiated a drying process on the substrate.

The drying model comprises a function correlating the time elapsed between the given time t and a time t0 at which the blood pool initiated a drying process on the substrate with a blood's diffusion coefficient, the blood's diffusion coefficient having a substantially constant value during the time elapsed between the given time t and a time t0 at which the blood pool initiated a drying process on the substrate for the same environmental conditions. The drying model comprises a blood's diffusion coefficient Dblood. Such blood's diffusion coefficient is expressed according to mathematical equation (1). In this example, the blood's diffusion coefficient was measured experimentally for a set of predetermined environmental conditions susceptible of representing the environmental conditions to which a blood pool may be commonly exposed. Hence, such blood's diffusion coefficient was available beforehand to users of the system, i.e., before dating the blood pool, and was stored in the above-mentioned database.

In this example of method, determining the drying front comprised determining the wet perimeter Pt and the wet area At. Both were determined by image processing a picture of the blood pool taken at the given time t. In this example of method according to the invention, numerical values of 0.16 m and 0.024 m2 were determined for the wet perimeter and the wet area, respectively.

The drying model of this example of method further comprises a function correlating the mass variation of the blood pool with time with the wet area of the blood pool at the given time t. The function is expressed according to equation (6′).

The evaporations of 4 blood pools having different masses (i.e., 1.75 g, 5.52 g, 10.10 g, 31.37 g) were plotted with time at 20% humidity, T=23° C.±1° C., on the same surface, a tile. For each of the 4 blood pools, Ki.Lk.L*0.5 of equation (1) was plotted with time which yielded a common plateau (see FIG. 4), yielding a substantially constant value of 10−9 m2·s−1 for the blood's diffusion coefficient Dblood.

In this example of method according to embodiments of the disclosure, the area A0 of the blood pool at a time t0 at which the blood pool initiated a drying process on the substrate and the environmental conditions to which the blood pool was exposed at the given time t, were determined. The determination of the area A0 was performed via image processing the picture of the blood pool taken at the given time t.

The method further comprised determining the blood pool's height. For this purpose, a statistical value of the blood pool's, i.e., the average value of blood pool's height measured for 30 blood pools having the same area of the blood pool at a time t0 at which the blood pool initiated a drying process on the substrate, was measured, for sets of predetermined environmental conditions. For example, at a temperature of 23±1° C. and a humidity of 20%, for a white tile as substrate, such measurement provides 1.44 mm (±0.19 mm) as the statistical value of the blood pool's height.

In this example of method, the function correlating the mass variation of the blood pool with time with the wet area of the blood pool at the given time t was expressed according to the following equation:

\(\begin{matrix}
{\frac{m_{t}}{m_{0}} = {1 - {\alpha\left\lbrack {1 - \frac{A_{t}}{A_{0}}} \right\rbrack}^{\beta}}} & (6)
\end{matrix}\)

This equation represents the variation of the normalized mass mt/m0 as a function of the normalized wet area At/A°, and involves correlation coefficients α and β.

The normalized mass of the blood pool was plotted with the normalized wet area of the blood pool at the given time t for 3 blood pools having different masses (i.e., 1.74 g, 5.52 g, 10.10 g) (see FIG. 5). A fitting curve was deduced from the superimposition of the 3 corresponding variations of the normalized mass mt/m0 as function of the normalized wet area. At/A0. Such fitting curve provided the correlation coefficients α and β.

The method allowed to determine the time elapsed δt between the given time t and a time t0 at which the blood pool initiated a drying process on the substrate. The time elapsed δt was calculated according to equation (8).

As numerical values, the blood's volume weight p was considered to be equal to 1,060 g/mL, the atmospheric pressure Pa was considered to be equal to 1.01325·105 Pa, the saturation vapor pressure of water Pw at the surface of the blood pool was obtained from standard charts, and the molecular diameter of water molecules was considered to be equal 0.343 nm.

Validation Test of the Method for Dating a Blood Pool Lying on a Substrate

Blood was spilled on a white tile at a time t0, i.e., at 10.17 am, thereby forming a blood pool initiating a drying process at time t0. A picture of the blood pool was then taken by a user via a system for dating blood pool according to the embodiments of the present disclosure while drying on the white tile, at a given time t, which was 6.54 pm, on the same day.

The user then proceeded to date the blood pool as follows.

Environmental conditions were determined. First, the GPS location of the blood pool was sent to a weather forecast virtual platform, and the temperature (23° C.) and humidity (20%) were Obtained in this manner. At this point, the user opened the dating software on the smartphone, and was asked by the dating software to upload the picture taken at 6.54 pm and to fill out blank fields corresponding to environmental parameters, i.e., the nature of the substrate, i.e., the tile. The user filled out the blank field. Thanks to the inclusion of the software image) within the dating software, the dating software then ran the image processing of the picture taken at time t and extracted At, Pt, A0 of the blood pool corresponding to the following values: A0=3.47E-03 m2; Pt=230E-03 in; At=3.17E-03 m2.

The calculation software compared these determined environmental conditions and the determined A0 with the sets of predetermined environmental and initial blood pool conditions impacting the drying of blood pools comprised in the database.

The calculation software, based on this comparison, determined numerical values associated to the determined environmental conditions for the correlation coefficients α and β, equal to 0.78 and 0.16, respectively, for the blood's diffusion coefficient Dblood, equal to 10−9 m2·s−1, and for the blood pool's height, equal to 1.44 ram.

For these values, the calculated elapsed time (5t was 498 min (8 h18 min), which turned out to be 19 min less than the exact measured elapsed time between time t and time t0 at which the blood pool was spilled on the white tile and initiated a drying process. This corresponded to a margin of error for the calculated elapsed time of less than 4% of the exact elapsed time.

In this example of the method, the picture of the blood pool was taken only once. However, the taking of a predetermined number of pictures of the blood pool could have been envisaged in order to obtain an average value of the elapsed time δE. For example, in one or more embodiments of the present invention, the taking of a predetermined number of pictures can be done. Each repetition of such step can be followed by the repetition of the calculation of said elapsed time, thereby leading to the calculation of the elapsed time a predetermined number of times. At that point, an average value of the elapsed time may be obtained. According to one or more embodiments, such repetitions can be performed in an automated way, at a predetermined time interval, e.g. 1 picture being taken per minute during 10 minutes. In the above-detailed description, only one set of predetermined environmental and initial blood pool conditions is shown, which is associated with the value of Dblood measured in FIG. 4, i.e., for a blood pool drying at 20% humidity, T=23° C.±1° C., on the same surface, a tile. However, the above-mentioned database, which comprises sets of predetermined environmental and initial blood pool conditions impacting the drying of blood pools associated with corresponding sets of measured blood pool parameters including blood's diffusion coefficients (Dblood) and blood pool's heights (h), may contain many more sets. FIG. 6 illustrates the interest of providing a rich database in the method according to one or more embodiments of the present disclosure. FIG. 6 is a diagram correlating KiLkL*0.5 with the percentage of water left in blood pools having the same mass (4.83±0.026 g) and drying at different humidities (30%, 40%, 50%), at the same temperature T=21° C.±1° C., on different substrates ‘surfaces (varnished or unvarnished parquet). The different values of blood's diffusion coefficients Dblood corresponding to the plateau values observed in FIG. 6 are presented in Table 1.

