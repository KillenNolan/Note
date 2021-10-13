# C#+EasyUI





var grid = $('#showData');
var options = grid.datagrid('getPager').data("pagination").options;
var curr = options.pageNumber;
var total = options.total;
var max = Math.ceil(total/options.pageSize);

