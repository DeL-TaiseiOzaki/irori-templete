---
name: vocabulary
description: "Review the page types and relations in .property/property.json against how the pages use them, and change them only as far as the person agrees: sharpen, add, fold, merge or retire one entry at a time."
---

# vocabulary

The only way `.property/property.json` changes. Run it when the person asks,
and suggest it when pages keep landing under the nearest type or an ordinary
link because nothing fitted. Leaving the vocabulary as it is is always an
acceptable outcome.

## Steps

1. **Evidence.** From `irori check`, the indexes and the pages' frontmatter:
   pages per `type` and per `rel`, names in use outside the vocabulary,
   relations from or to a type their declaration does not list, and index lines
   whose description does not fit the type they are filed under; by searching
   the bodies, the same relationship written as prose on several pages.
2. **Propose one change** with the whole vocabulary in view, the `avoid` list
   included: sharpen a definition the evidence shows is misread, add an entry
   the evidence needs and no entry covers, fold a name in use into the entry
   that already means it, merge two entries that mean the same, or retire one
   that no page uses.
3. **Check it before anyone sees it.** Discard it, and keep it in view so it
   does not come back, unless it differs in meaning, not only in spelling, from
   every entry and every avoided name; it names the pages it would change (at
   least three for a new entry) or the recurring question it answers; and every
   page stays conformant without moving. A new type names its `description`,
   its `heading` (unique) and its `folder`; a new relation its `from` and `to`.
4. **Stop** after three kept proposals, or sooner when new ones repeat earlier
   ones.
5. **Show** each kept proposal with its evidence and its exact edits: the
   entries in `.property/property.json` and the pages whose `type` or `rel`
   changes. The person accepts or turns down each one.
6. **Apply** what was accepted as one change. Add to `avoid` each name the
   change folded or merged away and each new name the person turned down, with
   what to use instead (another entry, or "a link"). Then run `irori index` and
   `irori check`.

## Boundaries

A change never moves a page: retyping changes its `type`, its index line and
its `generated`, not its path or its prose. Do not apply anything the person
did not accept.
