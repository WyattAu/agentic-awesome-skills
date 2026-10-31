1. **Use autoconfigure**; Migrate to `azure-monitor-opentelemetry-autoconfigure`
2. **Set meaningful span names**; Use descriptive operation names
3. **Add relevant attributes**; Include contextual data for debugging
4. **Handle exceptions**; Always record exceptions on spans
5. **Use semantic conventions**; Follow OpenTelemetry semantic conventions
6. **End spans in finally**; Ensure spans are always ended
7. **Use try-with-resources**; Scope management with try-with-resources pattern