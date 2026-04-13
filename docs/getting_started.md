# Getting Started

See [Dependencies](dependencies.md) first.

1. [Create a new repository from this template](https://github.com/new?template_name=jeff-template&template_owner=rjkiv), then clone it.

2. Rename `orig/GAMEID` to the game's ID. (For example, `373307D9` for _Dance Central 3_.)

3. Place your game's XEX in `orig/[GAMEID]`. If you have a `.map` file, place it here too.

4. Rename `config/GAMEID` to the game's ID and modify `config/[GAMEID]/config.yml` appropriately, using [`config.example.yml`](/config/GAMEID/config.example.yml) as a reference. If the game doesn't use RELs, the `modules` list in `config.yml` can be removed.

5. Update `VERSIONS` in [`tools/defines_common.py`](../tools/defines_common.py) with the game ID.

6. Run `python configure.py` to generate the initial `build.ninja`.

7. Run `ninja` to perform initial analysis.

If all goes well, the initial `symbols.txt` and `splits.txt` should be automatically generated. Though it's likely it won't build yet. See [Post-analysis](#post-analysis) for next steps.

## Using a `.map`

If the game has a `.map` file, it can be used to fill out `symbols.txt` and `splits.txt` automatically during the initial analysis.

Add the `map` key to `config.yml`, pointing to the `.map` file from the game disc (for example, `orig/[GAMEID]/main.map`).

Once the initial analysis is completed, `symbols.txt` and `splits.txt` will be generated from the map information. **Remove** the `map` field from `config.yml` to avoid conflicts.

## Using a `.pdb`

Similar to a `.map` file, a `.pdb` may alternatively be used to initialize a project with symbols, splits, and source file names. The setup is analogous to that for `.map` described above, but using the `pdb` key in the `config.yml` instead.

**For best results, please adhere to the following recommendations**:
* Add the lines `symbols_known: true` and `quick_analysis: true` to `config.yml`. If analysis fails, add `detect_objects: false` and `detect_strings: false` as well
* If your PDB analysis fails while displaying many warnings indicating that **object size lookup failed**, due to **unrecognized type records**, add `use_pdb_types: false` to your `config.yml` and retry
* If you have both a `.map` and `.pdb` available, rather than trying to apply them both, analyze with the `.pdb` first; if this is unsuccessful even after applying the suggestions above, opt for the `.map` (and [open an issue](https://github.com/rjkiv/jeff/issues))
* As mentioned above for `.map`, **remove** the `pdb` key from `config.yml` after initial analysis is successful. It is no longer needed for your project's configuration

## Post-analysis

After the initial analysis, `symbols.txt` and `splits.txt` will be generated. These files can be modified to adjust symbols and split points.
