CREATE TABLE public.extra_cr_map
(
    cr_number varchar(255) NOT NULL,
    cy varchar(255),
    created_at timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
    full_url varchar(255),
    tag varchar(255),
    artifact_group varchar(255),
    artifact_name varchar(255),
    artifact_version varchar(255)
);

CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = CURRENT_TIMESTAMP;
    RETURN NEW;
END;
$$ language 'plpgsql';

CREATE TRIGGER update_extra_cr_map_updated_at
BEFORE UPDATE ON public.extra_cr_map
FOR EACH ROW
EXECUTE FUNCTION update_updated_at_column();
