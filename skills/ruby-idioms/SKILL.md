---
name: ruby-idioms
description: Personal Ruby idiom preferences — return-value shapes. Use when writing or editing Ruby code, including ERB Ruby blocks.
---

# Ruby idioms (personal)

## Return values as expressions, not variables

Prefer the block-method form over build-a-local-then-return-it: the result becomes the expression's value and the trailing variable disappears.

Accumulating into a collection - prefer a purpose-built method when one exists (`filter_map`, `flat_map`, `sum`, `index_by`, `index_with`, `to_h`, `transform_values`); fall back to `each_with_object`:

Bad:

```ruby
targets = []
parents.each do |parent|
  target = index[parent]
  targets << target if target
end
targets
```

Good:

```ruby
parents.each_with_object([]) do |parent, targets|
  target = index[parent]
  targets << target if target
end
```

Transforming a hash's values - use `transform_values` (configure-in-place would be `tap`):

Bad:

```ruby
labels = names.dup
labels.each do |key, value|
  labels[key] = label(value)
end
labels
```

Good:

```ruby
names.transform_values { |value| label(value) }
```

## Judgment boundary

Do not force the idiom when it gets contorted: multiple exit points, heavily conditional accumulation, or a result built across unrelated branches. Readability wins - no linter or quality metric measures this.