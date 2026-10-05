# Neuromodulation & Behavior — Willuhn Group

**Netherlands Institute for Neuroscience · Amsterdam**

We study the neurobiology of reinforcement learning, decision making and
compulsive behavior in rodents: from everyday processes such as motivation,
habit formation and patience, to what goes wrong in compulsivity, craving and
impulsivity. Our work focuses on how the basal ganglia interact with the
cortex, and on how dopamine and serotonin shape these circuits, with a
translational link to obsessive-compulsive disorder and deep-brain stimulation.

🔗 [Lab website](https://nin.nl/research-groups/willuhn/)

---

## What you'll find here

This organization holds the lab's shared analysis code: tools for the
behavioral and circuit-specific brain-activity data we record, and the
analysis pipelines behind our publications.

Our experiments combine behavioral tasks in rats and mice with:
electrophysiology (single units and LFPs), fiber photometry, fast-scan cyclic
voltammetry, calcium imaging with miniaturized microscopes, optogenetics and
deep-brain stimulation.

### General tools
Reusable code that anyone can use for their own data.

| Repository | What it does | Language |
|---|---|---|
| [medpc-behavior](https://github.com/Willuhn-Group/medpc-behavior) | Read MedPC (Med Associates) files; trials, events, latencies, bouts | MATLAB |
| [matlab-utilities](https://github.com/Willuhn-Group/matlab-utilities) | General MATLAB helpers: project file listing and plotting utilities | MATLAB |
| *more coming soon* | | |

### Project repositories
The analysis code behind specific studies, archived with a DOI at publication.

| Repository | Study |
|---|---|
| [calcium_rats_gonogo](https://github.com/Willuhn-Group/calcium_rats_gonogo) | Prelimbic and infralimbic calcium activity during a go/no-go task in rats |
| [ephys-induced-polydipsia](https://github.com/Willuhn-Group/ephys-induced-polydipsia) | Electrophysiological markers of compulsive behavior: OFC and striatal LFPs and single units in schedule-induced polydipsia (rats) |

---

## Using our code

Each repository has its own README with requirements and an example you can
run on the included example data. If you use our code in your work, please
cite the release you used (see each repository's *Releases* page and
`CITATION.cff`).

## Contributing

Lab members and collaborators are welcome to contribute. Start with
[CONTRIBUTING.md](https://github.com/Willuhn-Group/.github/blob/main/CONTRIBUTING.md)
and the [lab code conventions](https://github.com/Willuhn-Group/.github/blob/main/LAB_CONVENTIONS.md).
