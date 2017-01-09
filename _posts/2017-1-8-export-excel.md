---
layout: post
title: Exportación a excel con Lamdas en dos patadas
excerpt_separator: <!--more-->
---

<!--more-->
{% highlight java %}
	public static Workbook createBook(List<OmDatosGeneralesDto> datos, List<SlickJson> colDefs ){
		Workbook wb = new XSSFWorkbook();

		CreationHelper createHelper = wb.getCreationHelper();
		
		Sheet sheet1 = wb.createSheet("Datos");
		
		CellStyle styleTit = UtilsExcelPoi.estiloCeldaTit(wb);
		CellStyle styleCab = UtilsExcelPoi.estiloCeldaCab(wb);
		CellStyle styleDat = UtilsExcelPoi.estiloCeldaDat(wb);
		CellStyle styleDatNumber = UtilsExcelPoi.estiloCeldaDatNumber(wb);
		
	    
		//TITULO
		Row rowTit = sheet1.createRow(0);
		Cell cellTit = rowTit.createCell(0);
		cellTit.setCellValue(createHelper.createRichTextString("Consulta Operaciones de Mercado"));
		cellTit.setCellStyle(styleTit);
		sheet1.addMergedRegion(new CellRangeAddress(0,0,0,8));
		
		// CABECERA
		Row rowHead = sheet1.createRow(1);
		int[] idx = { 1, 2 };
		colDefs.stream().forEachOrdered(item -> {
			
			Cell cab = rowHead.createCell(idx[0]++);
			cab.setCellStyle(styleCab);
			cab.setCellValue(createHelper.createRichTextString(item.getName()));
			
		});
		
		List<String> fields = colDefs.stream().map(field -> field.getField()).collect(Collectors.toList());
		
		datos.stream().forEachOrdered(dato ->{
			Row fila = sheet1.createRow(idx[1]++);
			int[] cellIdx = {1};
			fields.forEach(fieldName -> {
				Cell dataCell = fila.createCell(cellIdx[0]++);
				dataCell.setCellStyle(styleDat);
				try {
					Field privateField = OmDatosGeneralesDto.class.getDeclaredField(fieldName);
					privateField.setAccessible(true);
					String val = (String) privateField.get(dato);
					dataCell.setCellValue(val);
				} catch (Exception e) {
					//el campo no existe, es perfectamente posible
				}
			});
		});
		
		
		int numFields = fields.size();
		
			for (int i=1;i<numFields;i++){
				sheet1.autoSizeColumn(i);
			}

		

				return wb;
	}
{% endhighlight %}