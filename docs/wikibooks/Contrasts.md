If you want activated regions only, then you should apply a t-test (F
tests find differences in \'any\' direction while t-tests distinguish
between directions).

There are some predefined contrasts in SPM when you estimate a model
(e.g. the \"effects of interest\" f-contrast). If you hadn\'t defined
your control (or baseline) task then it would find areas activated for
stimulus A and also areas activated for stimulus B.
