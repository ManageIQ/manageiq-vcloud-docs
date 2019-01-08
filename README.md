To view current PDF version of the documentation, click [here](./CURRENT.pdf).

# VMware vCloud Director provider documentation (maintainer notes)

Repository contains documentation related to VMware vCloud Director provider. Documentation
should be compiled into PDF and shipped to customers.

**NOTE**: *README.md is intended for documentation maintainers only and should not be included
in the final PDF nor shipped to customers. All .adoc files, however, are actually
part of the documentation.*

## Compile into PDF
First navigate to [master.adoc](./vcd_installation_guide/master.adoc) and uncomment appropriate
variable set (either ManageIQ or CloudForms).

Then install [`asciidoctor-pdf`](https://asciidoctor.org/docs/asciidoctor-pdf) gem and use it like
this to compile .adoc documentation into a PDF file: 

```bash
bundle exec asciidoctor-pdf vcd_installation_guide/master.adoc -o ./PREVIEW.pdf
```

Command above will create file `./PREVIEW.pdf` that can be used as documentation preview.

## Documentation guides
When adding new sections to the documentation, we need to follow some guides in order to be
as compatible with other documentation as possible.

### Menu navigation
Menu navigation must be bolded and use `ITEM -> ITEM` syntax.

Correct: **User -> Settings -> Profile**

Wrong: 

- *User -> Settings -> Profile* (because it's italics)
- **User>Settings>Profile** (wrong separator)
- **User->Settings->Profile** (missing spaces)

### Referencing form fields
Any references must be bolded. Please do not use italics, nor quotes.

Correct: Select option **VMware WebMKS** in **Remote Console** section.

Wrong:

- Select option "VMware WebMKS" in **Remote Console** section. (because of "")
- Select option *VMware WebMKS* in **Remote Console** section. (because it's italics)

### Capitalize titles and lists
Titles must have all nouns capitalized. Furthermore, list items must have a form of sentence, not just plain
nouns.

Correct:
- Catalog, service dialog and catalog item preparation.
- Catalog item ordering with customization.
- Order approval, which results in actual provisioning to happen.

Wrong (because items are not nice sentences):
- catalog, service dialog and catalog item preparation
- catalog item ordering with customization
- order approval, which results in actual provisioning to happen

### Use variable for product names `{vcd}`, `{product-title}` etc.
It was a great mess with "vCloudDirector", "vCD", "VMware vCloudDirector" "vCloud" etc. therefore we
introduced it as an **asciidoc variable**. It's defined in [master.adoc](vcd_installation_guide/master.adoc)
along with some other handy variables that are available in all subsequent sections. Do use them!

Also, please pay attention to difference between:

- `:product-title:           Red Hat ManageIQ` (a product)
- `:product-appliance:       ManageIQ appliance` (a concrete virtual machine instance with MIQ, addressible by IP)
- `:vcd:                     VMware vCloud Director` (a product)
- `:vcd-host:                {vcd} host` (a concrete virtual machine instance with vCD, addressible by IP)
- `:vcd-provider:            {vcd} provider` (refers to vCD integration into MIQ - with inventoring, eventing and all)

Do not mix them because it makes documentation very difficult to follow then.

Correct: "{vcd-provider} supports establishing remote console against running virtual machines."
(value will be inserted upon PDF creation)

Wrong: "VMware vCloud Director provider supports establishing remote console against running virtual machines."
(because it hardcodes the "VMware vCloud Director provider" string)

Wrong: "vCloudDirector supports establishing remote console against running virtual machines."
(because it hardcodes utterly wrong string "vCloudDirector")

Wrong: "{vcd} supports establishing remote console against running virtual machines."
(because we refer to vCD as product, but we actually mean the CFME provider)

Wrong: "{vcd-host} supports establishing remote console against running virtual machines."
(because we refer to a concrete vCD instance, but we actually mean the CFME provider)

### Reference to other sections
Please do not use links when referencing to other sections of our documentation because links don't work
in PDF. Use anchors instead.

Correct: "More details on this matter can be found in \<\<VAppProvisioninVCD\>\> section."
(link will be inserted upon PDF creation)

Wrong: "More details on this matter can be found in link:vcd-vapp-provision.adoc[vApp Provisioning in vCloudDirector through Red Hat ManageIQ] section."
(because it uses direct link that won't work in PDF. Moreover, it hardcodes the link title. Using anchor uses
the document title.)

### Don't title images
Images don't have titles in CFME docs therefore we shouldn't be creating them either IMAO. Not sure on this
one. You can still add `alt` attribute.

Correct: image::../../images/vcd-vapp04-itemtype.png[alt="Select Orchestration stack"]
(`alt` attribute won't be rendered)

Wrong: image::../../images/vcd-vapp04-itemtype.png[title="Select Orchestration stack"]
(because image has title rendered)