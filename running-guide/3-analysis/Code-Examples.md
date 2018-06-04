<a name="top" />

[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort) > [**ANALYSIS**](Analysis) > [**CODE EXAMPLES**](Code Examples)

Below are some code examples (some in pseudo-code):

## Bremsstrahlung Suppression

```
bool suppressionFlags[<number of SCEPTAR>][<number of GRIFFIN>] = {...};

for each event:
	for-loop over GRIFFIN events {
		auto grif1 = fGrif->GetGriffinHit(index);
		grif1->SetIsBremSuppressed(false);
		for-loop over SCEPTAR events {
			auto scep = fScep->GetSceptarHit(index2);
			if(sceptar energy below threshold) continue;
			if(sceptar and griffin are in coincidence) {
				grif1->MakeBremSuppressed(suppressionFlags[scep->GetDetector()][grif1->GetArrayNumber()]);
			}
		}
		if(grif1->GetIsBremSuppressed()) continue;

		Fill Bremsstrahlungs suppressed histograms;
	}
```
