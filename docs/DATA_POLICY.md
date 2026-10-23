# Clinical research data policy

This repository contains **code and documentation only**.

**Never commit:** original DDR radiographs or video frames; filenames or tables containing patient identifiers; COCO datasets or annotations; trained weights; validation and prediction output; overlay images/videos; spreadsheets or reports containing clinical data.

The `AI-Hip Gate Analysis.zip` and the DDH dataset ZIP are supplied **only in the authenticated Google Colab session** or from appropriately restricted institutional storage. Do not put them in issues, commits, GitHub Releases, or pull requests.

The repository is currently **public**. GitHub `.gitignore` is a guardrail, not a security mechanism: users can still forcibly add ignored files. Review `git status` and filenames before committing.

For publication, note that current PA data contain 9 healthy patient videos (174 annotated frames), while ML has 10 (192 frames). Do not treat correlated frames as independent patient samples. Follow institutional IRB and image-use approvals.

Research prototype only; not validated for diagnostic or treatment decisions.
