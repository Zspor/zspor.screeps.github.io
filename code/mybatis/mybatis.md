#sql
##更新
###批量更新脚本
首先获取pojo和表的字段的名称
String pojoStr = "id,createBy,updateBy,isDel";
String tableStr = "id,create_by,update_by,is_del";
//将字段转为list
private static List<String> getStrings(String str) {
	String[]array = str.split(",");
	List<String> list = Lists.newArrayList();
	for (String s : array) {
		list.add(s);
	}
	return list;
}
List<String> pojoList = getStrings(pojoStr);
List<String> tableList = getStrings(tableStr);
/* 模板xml */
String xml = "<trim prefix=\"tablestr = case\" suffix=\"end,\">\n" +
				"                <foreach collection=\"list\" item=\"item\" index=\"index\">\n" +
				"                    <if test=\"item.pojostr != null\">\n" +
				"                        when id=#{item.id} then #{item.pojostr}\n" +
				"                    </if>\n" +
				"                    <if test=\"item.pojostr == null\">\n" +
				"                        when id=#{item.id} then bill_info.tablestr\n" +
				"                    </if>\n" +
				"                </foreach>\n" +
				"            </trim>\n";
				
String result = "";
for (int i = 0; i < pojoList.size(); i++) {
	String str1 = xml.replaceAll("pojostr",pojoList.get(i));
	String str2 = str1.replaceAll("tablestr",tableList.get(i));
	result += str2;
}
System.out.println(result);

<update id="updateBillInfoList" parameterType="list">
        update bill_info
        <trim prefix="set" suffixOverrides=",">
        把result放在这
        </trim>
        where id in
        <foreach collection="list" item="item" index="index" separator="," open="(" close=")">
            #{item.id}
        </foreach>
</update>
