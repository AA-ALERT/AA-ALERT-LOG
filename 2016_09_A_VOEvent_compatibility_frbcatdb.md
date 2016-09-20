# VOEvent compatibility in frbcatdb

The next update to the DB is to make it compatible with VOEvent (![Example: VOEvent](2016_09_A_VOEvent_example.xml) )

The very first "seeing" of a FRB will be in a VOEvent XML without a <Citations>
Other "seeings"" related to the same FRB will reference the previous VOEvent using <Citations>

We assume a VOEvent for a FRB is created when at least it has been detected (and measured) by at least one instrument.

Hence, a VOEvent of FRB (be it the first one or others) must contain info for 1 entry
in tables frb, observations, radio_obs_params and radio_measured_params (and notes)

For frb, observations and radio_obs_params the same entry may already exist
in which case we do not need to add new ones and we just reuse the old ones
(this means the current VOEvent is not the first seeing)

In this update we make changes in frbcatdb and frbcatweb.

frbcatdb needs to be ready to handle voevents. This also requires ivorn handling. Ivorns are identifiers for VOEvents and are also identifiers for entities that published VOEvents (Authors). We an authors table to the DB and each of the main tables (frbs, observations, rops and rmps) have a reference to the authors. We also add the VOEvent ivorn in the rmps table where we also store a copy of the VOEvent xml.

As part of this DB we also make table and column names consistent.

Additionally we add a radio_images_have_rmp table. Thus, a radio image can be linked to one or more radio_measured_params.

frbcatweb queries are updated accordingly to the described changes.
