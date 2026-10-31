Always use `FieldGroup` + `Field`; never raw `div` with `space-y-*`:
Use `FieldSet` + `FieldLegend` for related checkboxes, radios, or switches; not `div` with a heading:
Both attributes are needed; `data-invalid`/`data-disabled` styles the field (label, description), while `aria-invalid`/`disabled` styles the control.