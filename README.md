# scarr-file-converters

This repository is a collection of file converter scripts, to convert from X to .zarr, specifically within the context of side-channel analysis. This effort is intended to help others to make use of [SCARR](https://github.com/decryptofy/scarr).

We anticipate to support the following file formats soon:

* .npy to .zarr (as used, e.g., by the [ChipWhisperer](https://github.com/newaetech/chipwhisperer) project)
* .ets/.h5 to .zarr (as used, e.g., by [SCARED](https://gitlab.com/eshard/scared) and [LASCAR](https://github.com/Ledger-Donjon/lascar))
* .db to .zarr (as used, e.g., by [ot-sca](https://github.com/lowRISC/ot-sca/blob/master/capture/project_library/ot_trace_library/trace_library.py))

For the file conversion, please take the following general advice into consideration:

* Compression:
   * You can choose to leave your data uncompressed, or compress it with lz4 or lz4hc (clevel 9)
   * (u)int8 --> no specific conserations regarding compression
   * (u)int16 --> if this contains a 12 bit value, _please make sure it is LSB-aligned_, otherwise compression is not optimal
* Chunking:
   * A chunking of (5000,1000) is suggested as a default value and will give you good performance for many real-world scenarios
   * Depending on your sample rate, switching activity of your DUT, and access pattern, other values such as (5000, 500) could be better
   * Zarr is currently being optimized for higher speed when rechunking, which in turn, will optimize this file conversion step
   * If file conversion speed matters to you, use (5000, x) whereas x = trace length, i.e., not chunking the 2nd dimension
   * When leaving the 2nd dimension unchunked, indexing will not be as fast

These scripts are otherwise not optimized for performance but should support out-of-core processing, to convert data sets larger than the available memory. We are more than happy about additional contributions (e.g., .trs to .zarr).

# Contributing (inbound=outbound)

Consistent with Section D.6. of the [GitHub Terms of Service](https://docs.github.com/en/site-policy/github-terms/github-terms-of-service) as of November 16, 2020, and Section 5. of the [Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0.txt), the project maintainer for this project accepts contributions using the inbound=outbound model. When you submit a pull request to this repository (inbound), you are agreeing to license your contribution under the same terms as specified under [License](https://github.com/decryptofy/scarr-file-converters/blob/main/LICENSE) (outbound).

# Legal

This is an open source project. Contributions you make to this public repository are completely voluntary. When you submit an issue, bug report, question, enhancement, pull request, etc., you are offering your contribution without expectation of payment, you expressly waive any future pay claims against the maintainers of this repository related to your contribution, and you acknowledge that this does not create an obligation on the part of the maintainer of any kind. Furthermore, your contributing to this project does not create an employer-employee relationship between the maintainers of this repository and the contributor.
