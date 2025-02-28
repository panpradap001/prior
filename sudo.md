BEGIN

    LOAD raw_data FROM "rawdata.txt"

    SET node_mapping = MAP()
    SET nodes = LIST()
    SET address_in = LIST()     //ส่งจากโหนดไหน
    SET address_out = LIST()    //รับ

    FOR EACH node IN raw_data["nodes"] DO //วนซ้ำผ่านแต่ละ node ใน raw_data["nodes"]
        SET node_mapping[node.id] = INDEX(node)
        ADD node.type TO nodes
        ADD "" TO address_in    //ค่าเริ่มต้น
        ADD "" TO address_out

    FOR EACH edge IN raw_data["edges"] DO
        SET source_index = node_mapping[edge.source]//ดูว่า sourceใน node_mapping  อะไรแล้วเก็บไว้
        SET target_index = node_mapping[edge.target]
        SET address_out[source_index] = edge.target//ถ้า data ที่ out ก็หมายถึงของมูลที่มี target ว่าจะไปที่ไหน
        SET address_in[target_index] = edge.source

    SET Nodes = nodes
    SET AddressIn = LIST(address_in[node_mapping[n]] FOR EACH n IN node_mapping) 
    SET AddressOut = LIST(address_out[node_mapping[n]] FOR EACH n IN node_mapping)

    DISPLAY "Nodes =", Nodes
    DISPLAY "AddressIn =", AddressIn
    DISPLAY "AddressOut =", AddressOut

END
