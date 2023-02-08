To unlock reissuance token (RT), first-stage covenant code must ensure the following:

- Transaction contains exactly two inputs

    * First input is blinded
    * Second input is unblined, and contains L-BTC (to pay the fee)

- Transaction contains three or four outputs

    * Output script of the first output is the same as the output script of the first input
    * Second output is ublinded Synth that goes to second stage covenant
    * Next output (may be absent) is unblinded L-BTC (change from the fee)
    * Last output is the transaction fee (L-BTC)

First stage covenant code must also ensure that second-stage covenant script is the one
that enforces the conditions that correspond to the oracle-attested price rate with timestamp that
were presented to the first-stage covenant. To check for that, first-stage covenant must (partially)
construct the second-stage covenant script, create output script from it (tweaked taproot key),
and compare it with the taproot key in the second output

Because the transaction contains only two inputs, the second input containing L-BTC, the first input
will always be RT. This is because it is RT that we are locking in the covenant, and therefore
it must be present in the inputs

Because RT is present in the inputs, it must be also present in the outputs for the transaction
to be valid. We check that all outputs except the first are unblinded and are not RT, therefore
the first output must be RT

First output may or may not be blinded, we only check that its output script is equal to the output
script of the first input. Because there is only one RT input and one RT output, the amount of
input and output must be the same for the transaction to be valid

There is only one Synth output, and therefore all the issued Synth must be sent to the second-stage
covenant via this second output. The first-stage covenant code can therefore use the amount of
the second output (that is unblinded) to reference the amount issued, even if the issuance data
in the first input is confidential
