# Normalize frbcatdb

After creating the frbcatdb GitHub repository (imported from BitBucket) and after initial discussion with Emily
we do an update to frbcatdb to normalize the database and to add N:N relationships
between the main tables (frbs, observations, radio_obs_params and radio_measured_params)
and the references table via `*_have_refs` tables
