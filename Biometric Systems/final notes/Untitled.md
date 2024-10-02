#### Problems of biometric systems:
- wide intra-class variations
	- maybe different facial expression, different light, different view point...
- very small inter-class variations
	- two different person very similar (i.e. twins)

- possible spoofing attacks, in different moments
	![[Pasted image 20241002181936.png]]

- [non universality](LEZIONE2_Indici_di_prestazione.pdf#page=6&selection=0,10,0,26&color=yellow|LEZIONE2_Indici_di_prestazione, p.6)
	- e.g. people with no voice, people with cataract, people with poor fingerprints...

Most difficult traits to exploit:
- retina fundus
- behavioral traits (i.e. way of walking)
- handwriting

### [What to compare?](LEZIONE2_Indici_di_prestazione.pdf#page=8&selection=0,10,0,26&color=yellow|LEZIONE2_Indici_di_prestazione, p.8)
- **Sample
	- raw captured data
- **Hand-crafted features**
	- manually engineered by the data scientist and extracted from samples
	- can also be substituted with **embeddings**: features automatically extracted by deep architectures
- **Template**
	- collection of features extracted from the row data, examples:
		- a histogram representing the frequencies of relevant values in the image (e.g. greylevel values)
		- a vector of values each representing a relevant measure (e.g. Bertillon measures)
		- time series of acceleration values (one per axis)
		- a set of triplets as for relevant fingerprint points representing the coordinates of the points and the direction of the tangent to the ridge in that point.


> [!PDF|red] [[LEZIONE2_Indici_di_prestazione.pdf#page=8&selection=11,1,14,16&color=red|LEZIONE2_Indici_di_prestazione, p.8]]
> > Hand-crafted features
> 
> not the template of the entire biometric system.


### Comparing templates
- Euclidian distance
- Cosine similarity
	- cosine of the angle between two vectors
- Pearson correlation
- Bhattacharyya distance


> [!PDF|yellow] [[LEZIONE2_Indici_di_prestazione.pdf#page=9&selection=8,0,10,31&color=yellow|LEZIONE2_Indici_di_prestazione, p.9]]
> > or cosine similarity may provide either a distance measure or a similarity measure
> 
> shows "more stuff" than Euclidian distance, such as orientation ecc.. Shows how templates are similar to eachother. While distance shows how templates are... distant!

> [!PDF|yellow] [[LEZIONE2_Indici_di_prestazione.pdf#page=10&selection=3,1,4,21&color=yellow|LEZIONE2_Indici_di_prestazione, p.10]]
> > (Pearson) Correlation
> 
> how signals are similar to eachother. Often used to compare fingerprints, by computing the correlation between two fingerprints.

Histograms needs other ways to be compared.

The same happens with time series: speed for example may speed the final outcome of the time series, even if the patterns are the same.

So what do we do?
sometimes we use correlation, but Dynamic time Warping is the most used.

![[Pasted image 20241002135922.png]]

each point is paired with the most convenient one. It's not necessaty that points corresponds to the same instant in time.

if using deep learning we should use the architecture to extract the embeddings (for both gallery and probe templates).

//
after normalization in range [0, 1] we will have that distance = 1 - similarity.

#### Possible errors: verification
- Genuine Match (GM, GA): the claimed identity is true and subject is accepted
- False Rejection (FR, FNM, type I error): claimed identity is true but the subjet is rejected
- Genuine Reject (GR, GNM): an impostor is rejected
- False Acceptance (FA, FM, type II error): an impostor is accepted :/

It's important to define a good threshold.
If too high we will get a lot of false acceptance. If too low we will get a lot of false rejection!

When computing rates:
- False Rejection Rate (FRR) is the number of FR divided by ONLY the number of GM+FR.
	- in fact, GM + FR have the same denominator and sum up to 1.
- False Acceptance Rate is the number of FA divided by FA + GR

Equal Error Rate is the value at a specific threshold, where FAR and FRR are the same value.

two synthetic metrics could be ERR and area below ROC curve.

(we might have more templates for the same person to address inter-class variation.
Of course templates should be different, not computed i.e. by frames of the same video, as some of them could be blurred and close frames are exactly the same!)

> [!PDF|yellow] [[LEZIONE2_Indici_di_prestazione.pdf#page=20&selection=119,0,119,4&color=yellow|LEZIONE2_Indici_di_prestazione, p.20]]
> > When
> 
> in false acceptance we can have two possible scenarios
> - pj does not belong to the gallery (most trivial)
> - pj belongs to an enrolled subject but the probe claimed another identity, not the real one.

