##  Market Clear- Credit Rating Platform for Traders               


The objective of this project is to examine Retailer and provides the report to the Distributor.
The project involves the development of Business making System for Distributor and Retailer (Market Clear) System. It is an application development project starting with requirement gathering, application design and development, testing, implementation and also post implementation support. The entire system is developed using iterative model. The project has been undertaken to satisfy the following objectives:
 The project has been undertaken to satisfy the following objectives:
•	Notify Retailer electronically of coming due or overdue Class Surveys and Outstanding Recommendations, Statutory Surveys and Additional Findings, and overdue items.
•	Conserve Classification and Documentation Center time and labour resources. 
•	Simplify Suspension or Overdue notification review process 


### A Demo API


```markdown
@Controller
@RequestMapping("/qr")
public class QRCodeRestService {
	Logger logger = LoggerFactory.getLogger(QRCodeRestService.class);
	@Value("${qrcode.paper.size}")
	private String qrcodePageSize;
	@Value("${qrcode.folder.path}")
	private String qrcodeFolderPath;
	@Autowired
	QRCodeService qrCoder;
	

	@RequestMapping(value = "/rc", method = RequestMethod.GET, produces = MediaType.TEXT_PLAIN_VALUE)
	public @ResponseBody String generateRandonNumberForQRCode(){
		logger.info("generateRandonNumberForQRCode called ");
		return qrCoder.getRandonQRCode();
	}
	
	@RequestMapping(value = "/qc", method = RequestMethod.GET, produces = MediaType.IMAGE_PNG_VALUE)
	public @ResponseBody byte[] getRandomQRCodeImage(){
		logger.info("getRandomQRCodeImage called ");
		return qrCoder.getRandomQRCodeImage();
	}
	
	@RequestMapping(value = "/qcp/{count}", method = RequestMethod.GET, produces=MediaType.TEXT_PLAIN_VALUE)
	public @ResponseBody String getQRCodesInFolder(@PathVariable int count) throws Exception {
		logger.info("getQRCodesInFolder called for count {}", count);

		try {
			Path path =null;
			for(int i=0; i<count; i++){
				path = Paths.get(qrcodeFolderPath+"QRCode_"+System.nanoTime()+".png");
	            Files.write(path, getRandomQRCodeImage());
			}
		} catch (Exception e) {
			logger.error("Exception in getting QR Codes in folder",e);
			return "ERROR in creating QR Codes";
		}
		
		return count+" QR Codes created and saved in folder "+qrcodeFolderPath;
	}

	
	@RequestMapping(value = "/qcp/{count}/{pageSize}", method = RequestMethod.GET)
	public void getQRCodesInPDF(@PathVariable int count,@PathVariable String pageSize,HttpServletResponse response) throws Exception {
		logger.info("getQRCodesInPDF called for count {} and page size {}", count, pageSize);
		response.setContentType("application/pdf");
		response.addHeader("Content-Disposition","attachment; filename=mSecure-QRCodes-"+pageSize+".pdf" );
		
		try {
			//byte[] mergedImageBytes=getRandomQRCodeImage();
			if(pageSize.equalsIgnoreCase("NA")){
				pageSize=qrcodePageSize;
			}
			qrCoder.createPDFFileForQRCodes(pageSize.toUpperCase(), count, response.getOutputStream());
			response.getOutputStream().flush();
		} catch (IOException e) {
			logger.error("Exception in getting PDF",e);
			throw new Exception("Exception in getting PDF"); 
		}
	}
	
	
	@RequestMapping(value = "/vbd/{blockId}", method = RequestMethod.GET, produces = MediaType.TEXT_PLAIN_VALUE)
	public @ResponseBody String viewBlockChainData(@PathVariable String blockId){
		logger.info("getTestBlock called ");
		return "Hi this is a sample block from id "+blockId;
	}


```

