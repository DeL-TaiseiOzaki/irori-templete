---
id: <scope>/<ULID>
title: <name>
type: person | org | repo
status: active
created: <YYYY-MM-DD>
sensitivity: internal
derived_from: []
---

# <name>

What stays true. Add the matching row to `Knowledge_Base/ontology/entities.csv`
in the same change, with `group` set to the type and `note` set to this file's
path. If a higher scope already owns this identity, keep this file to your local
detail and give the row a `same_as` relation to the upper scope's id.
