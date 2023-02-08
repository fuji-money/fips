To unlock reissuance token (RT), the contract must ensure the following:

- Transaction contains two inputs, the second of which is unblinded L-BTC
- Transaction contains four or five outputs

    * First output is blined (supposed to be RT), but its outputs script
      is the same as the output script of the first input
    * Second output is ublinded Synth (subject for price checks)
    * Third output is unblinded L-BTC that goes to Safe covenant (subject to price checks)
    * Next output (may be absent) is unblinded L-BTC (change from the fee)
    * Last output is fee

Because the transaction contains only two inputs, one of which must be L-BTC. We check that the
second input is L-BTC, the first input must RT. This is because it is RT that we are locking in the
covenant, and it has to be present in the inputs.

The covenant will check that the output script of the first input is the same as output script of the first output.
Because all other outputs are unblined and are not RT, the only blinded (first) output must be the RT output.

The price check code can ensure that there's an issuance data on the first input,
and thus can check the issuance amount. The second output is the Synth that is not in the inputs
(by the above logic), therefore the price check code can check the amount of second input against
the issuance amount.

There must be L-BTC output that goes to Safe covenant, and its amount will be checked to correspond to the price.
The extra L-BTC output can be any amount, and the covenant will not be concerned with its amount (but must check that
it is unblinded L-BTC)

And of course the last output and total number of output must be checked 
