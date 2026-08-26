# Master data

Master data defines the relatively stable business records referenced by Food & Beverage production and operational data submitted to Pulse.

These records describe the production structure, products, materials, business partners, and working context used by runs, batches, readings, consumption, maintenance, complaints, and other operational records.

Register referenced master data before submitting records that depend on it.

## Available master data

### Production structure

| Resource | Purpose |
| --- | --- |
| [**Site**](site.md) | Represents a physical operating location. |
| [**Production line**](production-line.md) | Represents a production line within a site. |
| [**Machine**](machine.md) | Represents a machine, equipment asset, component, or sensor. |
| [**Vessel**](vessel.md) | Represents a tank, silo, or other process vessel. |

### Products and materials

| Resource | Purpose |
| --- | --- |
| [**Product**](product.md) | Represents a finished product or other output tracked in Pulse. |
| [**Recipe**](recipe.md) | Represents a versioned recipe used during production. |
| [**Material**](material.md) | Represents an ingredient, packaging material, chemical, utility, or other material consumed in production. |
| [**Cost line**](cost-line.md) | Represents a non-material cost such as overtime, subcontracting, or disposal. |

### Business context

| Resource | Purpose |
| --- | --- |
| [**Supplier**](supplier.md) | Represents a supplier associated with materials and other inputs. |
| [**Customer**](customer.md) | Represents a customer associated with Food & Beverage operations. |
| [**Shift**](shift.md) | Represents a production shift used as calendar context. |
| [**Crew**](crew.md) | Represents a production crew used to attribute work and labor activity. |

See [Types and attributes](types-and-attributes.md) for guidance on extensible classifications and additional source-system metadata.