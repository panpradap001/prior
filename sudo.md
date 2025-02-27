BEGIN

    #โหลดไฟล์ Raw Data และเก็บเป็น list
    FUNCTION load_raw_data(file_path):
        OPEN file and read lines
        RETURN raw_data_list

    #สร้างโครงสร้าง Nodes และ Address Mapping เป็น Array Index-Based 
    FUNCTION build_node_graph():
        DEFINE nodes, addressIn, addressOut
        RETURN {node: {"addressIn": addressIn[i], "addressOut": addressOut[i]} FOR i IN range(nodes)}

    #เอาข้อมูลจากมาผ่านโหนดจน output
    FUNCTION process_data(raw_data, graph):
        FOR each record IN raw_data:
            SET node = "Input"
            WHILE node is not "Output":
                APPEND (record, node, graph[node]["addressOut"]) TO processed_data
                UPDATE node = graph[node]["addressOut"]
        RETURN processed_data

    #ดูว่าส่งข้อมูลไปโหนดไหนต่อ
    FUNCTION send_data(processed_data):
        FOR each (data, from_node, to_node) IN processed_data:
            PRINT "Sending", data, "from", from_node, "to", to_node


    raw_data = load_raw_data("rawdata.txt")
    graph = build_node_graph()
    processed_data = process_data(raw_data, graph)
    send_data(processed_data)

END
