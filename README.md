### Example of PDF File Compression using iText 7

<P>using iText.Kernel.Pdf;
using iText.Kernel.Utils;
using System.IO;

namespace BMC<br />
{
    class iText_API
    {        
        public static void compressPDF(string existingFileFullPath, string outputFileFullPath)
        {
            // use iText to reduce PDF file size
            using (var memoryStream = new MemoryStream())
            {
                using (PdfReader pdfReader = new PdfReader(existingFileFullPath))
                {
                    using (PdfDocument SourceDocument1 = new PdfDocument(pdfReader))
                    {
                        WriterProperties properties = new WriterProperties().SetCompressionLevel(CompressionConstants.BEST_COMPRESSION);
                        using (PdfWriter pdfWriter = new PdfWriter(memoryStream, properties))
                        {
                            using (PdfDocument pdfDocument = new PdfDocument(pdfWriter))
                            {
                                PdfMerger merge = new PdfMerger(pdfDocument);
                                merge.Merge(SourceDocument1, 1, SourceDocument1.GetNumberOfPages());
                                merge.Close();
                                SourceDocument1.Close();

                                byte[] result = memoryStream.ToArray();

                                File.WriteAllBytes(outputFileFullPath, result);
                            }
                        }
                    }
                }
            }
        }
    }
}


</p>

