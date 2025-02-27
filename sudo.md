BEGIN

    #โหลดไฟล์ Raw Data
    FUNCTION load_raw_data(file_path):
        OPEN file and read lines
        RETURN raw_data_list

    #สร้างโครงสร้าง Nodes และ Address Mapping (Array Index-Based)
    FUNCTION build_node_graph():
        DEFINE nodes, addressIn, addressOut
        RETURN {node: {"addressIn": addressIn[i], "addressOut": addressOut[i]} FOR i IN range(nodes)}

    #ประมวลผลข้อมูลตามลำดับ Nodes
    FUNCTION process_data(raw_data, graph):
        FOR each record IN raw_data:
            SET node = "Input"
            WHILE node is not "Output":
                APPEND (record, node, graph[node]["addressOut"]) TO processed_data
                UPDATE node = graph[node]["addressOut"]
        RETURN processed_data

    #ส่งข้อมูลไปยัง Nodes
    FUNCTION send_data(processed_data):
        FOR each (data, from_node, to_node) IN processed_data:
            PRINT "Sending", data, "from", from_node, "to", to_node

    #เรียกใช้ฟังก์ชันหลัก
    raw_data = load_raw_data("rawdata.txt")
    graph = build_node_graph()
    processed_data = process_data(raw_data, graph)
    send_data(processed_data)

END
