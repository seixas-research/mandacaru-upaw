# Mandacaru UPAW Pseudopotentials

Unitary projector augmented-wave datasets (UPAW-LCAO) for
[Mandacaru](https://github.com/seixas-research/mandacaru). UPAW-LCAO is the
PAW-LCAO construction with a vanishing norm deficit: the smooth partial waves
carry the full all-electron norm, so the overlap correction is zero, the
transformation is unitary and the pseudo states are orthonormal
(D. Ivanov *et al.*, arXiv:2408.03159). Mandacaru generates the datasets from
scratch with `mandacaru.pseudopotentials.paw.generate_upaw`.

## Contents

This repository holds **no datasets yet**. Datasets would go one file per
element in `lda/` (and `pbe/` for a PBE set), which is where Mandacaru reads
them.

A library here is optional. Without one, Mandacaru generates each UPAW-LCAO
dataset on demand, in a few seconds per element, and caches it for the
process.

## Using the datasets

```bash
git clone https://github.com/seixas-research/mandacaru-upaw
mandacaru --set-upaw /path/to/mandacaru-upaw   # writes MANDACARU_UPAW_PATH
# open a new terminal, then
mandacaru --pseudo-status
```

The family is selected as a basis, `basis="UPAW-LCAO"` (alias
`"unitary-paw-lcao"`):

```python
from ase.build import molecule
from mandacaru import Mandacaru

atoms = molecule("H2O")
atoms.center(vacuum=4.0)
atoms.calc = Mandacaru(method="adapt-vqe", basis="UPAW-LCAO", h=0.25)
atoms.get_potential_energy()
```

With `MANDACARU_UPAW_PATH` unset, this runs with datasets generated on the
spot. With it set, Mandacaru reads `$MANDACARU_UPAW_PATH/lda/`, and still
generates any element the library lacks.

## Generating the datasets

With a Mandacaru development install and `MANDACARU_UPAW_PATH` set:

```bash
mandacaru-build --pp UPAW --relativistic --xc LDA --all --workers 7 --check --ghosts flag --install
```

`--install` writes into `$MANDACARU_UPAW_PATH/lda/`. The construction, the
ghost-state and scattering checks, and the recording of defects are those of
PAW-LCAO (see [mandacaru-paw](https://github.com/seixas-research/mandacaru-paw)),
at a norm deficit of zero.

## License

MIT, see [LICENSE](LICENSE).
