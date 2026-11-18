---
name: ruby-idioms
description: Personal Ruby idiom preferences. Use when writing or editing Ruby code, including ERB Ruby blocks.
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

## Class methods: visibility at the site

Class methods are `def self.foo` — never `class << self`. Positional `private` makes visibility an accident of placement: a helper added above the macro is silently public. Private class methods carry the visibility inline:

Bad:

```ruby
class << self
  def fetch(uri)
  end

  private

  def key_for(url)
  end
end
```

Good:

```ruby
def self.fetch(uri)
end

private_class_method def self.key_for(url)
end
```

## RuboCop directives: `disable-next`, never `disable-next-line`

A single-line suppression is `# rubocop:disable-next <Cop>`. There is no `disable-next-line` in RuboCop — the directive modes are `disable`, `enable`, `todo`, `push`, `pop`, `disable-next`, `todo-next`, `enable-next`, `next` (RuboCop 1.92, `directive_comment.rb`) — so the `-line` spelling writes an unrecognized directive: the warning fires and the offense still trips. The mistake recurs because other linters (ESLint and friends) use a `-line` suffix; RuboCop does not, at any version.

## Judgment boundary

Do not force the idiom when it gets contorted: multiple exit points, heavily conditional accumulation, or a result built across unrelated branches. Readability wins - no linter or quality metric measures this.