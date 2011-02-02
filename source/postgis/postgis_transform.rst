.. _postgis_transform:


#########
transform
#########

SRID ::

    select srid from geometry_columns where f_table_name='cimetiere';




Transformer en lambert 93 dans la meme table ::
    
    SELECT addGeometryColumn( 'cimetiere', 'geom_rgf93', 2154, 'POLYGON', 2);

        CONSTRAINT enforce_dims_geom CHECK (st_ndims(geom) = 2),
        CONSTRAINT enforce_dims_geom_rgf93 CHECK (st_ndims(geom_rgf93) = 2),
        CONSTRAINT enforce_geotype_geom CHECK (geometrytype(geom) = 'POLYGON'::text OR geom IS NULL),
        CONSTRAINT enforce_geotype_geom_rgf93 CHECK (geometrytype(geom_rgf93) = 'POLYGON'::text OR geom_rgf93 IS NULL),
        CONSTRAINT enforce_srid_geom CHECK (st_srid(geom) = 27563),
        CONSTRAINT enforce_srid_geom_rgf93 CHECK (st_srid(geom_rgf93) = 2154)
    
    UPDATE cimetiere SET geom_rgf93 = transform(geom,2154);

    select astext(geom), astext(geom_rgf93) from cimetiere
    
        "POLYGON((785186.97844459 155628.164220465,
                785245.927801345 155601.446166684, ...
        "POLYGON((831753.180418834 6287843.04161093,
                  831811.956095086 6287815.89024952 ...


Transformer en mercator et envoi en fichier kml  ::

    select cimetierelib, ST_asKML(transform(geom, 3385)) as geom from cimetiere


Transformer un point ::

    select astext(transform(setsrid(geometryfromtext
        ('POINT(4.632438 43.684858)'),4326),27563));
        
                POINT(785074.087673277 156458.572267362)
        
    select astext(transform(setsrid(geometryfromtext
        ('POINT(4.632438 43.684858)'),4326),27563));

                POINT(785074.087673277 156458.572267362)    

    