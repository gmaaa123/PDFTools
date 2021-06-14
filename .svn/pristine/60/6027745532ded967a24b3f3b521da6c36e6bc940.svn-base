using System;
using System.Collections.Generic;
using iTextSharp.text;
using iTextSharp.text.pdf;
using System.Text;

namespace PDFTools
{
    /// <summary>
    /// 文件名:PdfExtractorUtility/
    /// 文件功能描述:处理PDF文件/
    /// 版权所有:Copyright (C) EXT.AZHANG/
    /// 创建标识:2021.6.2/     /// 修改描述:/
    /// </summary>
    class PdfExtractorUtility
    {
        /// <summary> 
        /// 从已有PDF文件中拷贝指定的页码范围到一个新的pdf文件中 使用pdfCopyProvider.AddPage()方法
        /// </summary>
        /// <param name="sourcePdfPath">文件路径+文件名</param>
        public void SplitPDF(string sourcePdfPath, string outputPdfPath, int startPage, int endPage)
        {
            PdfReader reader = null;
            Document sourceDocument = null;
            PdfCopy pdfCopyProvider = null;
            PdfImportedPage importedPage = null;
            try
            {
                reader = new PdfReader(sourcePdfPath);
                sourceDocument = new Document(reader.GetPageSizeWithRotation(startPage)); pdfCopyProvider = new PdfCopy(sourceDocument, new System.IO.FileStream(outputPdfPath, System.IO.FileMode.Create));
                sourceDocument.Open();
                for (int i = startPage; i <= endPage; i++)
                {
                    importedPage = pdfCopyProvider.GetImportedPage(reader, i); pdfCopyProvider.AddPage(importedPage);
                }
                sourceDocument.Close();
                reader.Close();
            }
            catch (Exception ex) { throw ex; }
        }


        /// <summary> 
        /// 将PDF文件分割成单页
        /// </summary>
        /// <param name="sourcePdfPath">文件路径+文件名</param>
        public void Split2SinglePage(string sourcePdfPath)
        {
            PdfReader reader = null;
            try
            {
                string fileNameWithoutExtension = System.IO.Path.GetFileNameWithoutExtension(sourcePdfPath);
                string outputPdfFolder = System.IO.Path.GetDirectoryName(sourcePdfPath);
                reader = new PdfReader(sourcePdfPath);

                for (int i = 1; i <= reader.NumberOfPages; i++)
                {
                    PdfCopy pdfCopyProvider = null;
                    PdfImportedPage importedPage = null;
                    Document sourceDocument = null;
                    string outputPdfPath = outputPdfFolder + "\\" + fileNameWithoutExtension + "_" + i + ".pdf";
                    sourceDocument = new Document(reader.GetPageSizeWithRotation(i));
                    pdfCopyProvider = new PdfCopy(sourceDocument, new System.IO.FileStream(outputPdfPath, System.IO.FileMode.Create));
                    sourceDocument.Open();
                    importedPage = pdfCopyProvider.GetImportedPage(reader, i);
                    pdfCopyProvider.AddPage(importedPage);
                    sourceDocument.Close();
                }

                reader.Close();
            }
            catch (Exception ex) { throw ex; }
        }


        /// <summary> 
        /// 将PDF文件平均分割成多个文件，无法分尽，剩余页数就加到最后一个文档
        /// </summary>
        /// <param name="sourcePdfPath">文件路径+文件名</param>
        /// <param name="count">需要生成的文档数量</param>
        public void Split2AveragePage(string sourcePdfPath, int count)
        {
            PdfReader reader = null;
            try
            {
                string fileNameWithoutExtension = System.IO.Path.GetFileNameWithoutExtension(sourcePdfPath);
                string outputPdfFolder = System.IO.Path.GetDirectoryName(sourcePdfPath);
                reader = new PdfReader(sourcePdfPath);
                // int page = (reader.NumberOfPages / count);
                // 计算每个文档的页数，总是舍去小数
                int page = (int)Math.Floor((double)(reader.NumberOfPages) / (double)(count));
                int startPage = 1;
                int endPage = 1;

                LogUtil.WriteLog("每个文档的页数：" + page.ToString());

                for (int i = 1; i <= count; i++)
                {
                    string outputPdfPath = outputPdfFolder + "\\" + fileNameWithoutExtension + "_" + i + ".pdf"; ;

                    if (i == 1)
                    {
                        startPage = 1;
                        endPage = page;

                    }
                    else
                    {

                        startPage = endPage + 1;
                        endPage = startPage + page - 1;
                    }

                    if (startPage > reader.NumberOfPages)
                        break;

                    if (endPage > reader.NumberOfPages)
                        endPage = reader.NumberOfPages;

                    if (i == count)
                        endPage = reader.NumberOfPages;

                    LogUtil.WriteLog(outputPdfPath + " > " + startPage.ToString() + "-" + endPage.ToString());
                    SplitPDF(sourcePdfPath, outputPdfPath, startPage, endPage);

                }

                reader.Close();
            }
            catch (Exception ex) { throw ex; }
        }



        /// <summary> 
        /// 将PDF文件按文档固定页数割成多个文件
        /// </summary>
        /// <param name="sourcePdfPath">文件路径+文件名</param>
        /// <param name="page">每个文档页数</param>
        public void Split2Page(string sourcePdfPath, int page)
        {
            PdfReader reader = null;
            try
            {
                string fileNameWithoutExtension = System.IO.Path.GetFileNameWithoutExtension(sourcePdfPath);
                string outputPdfFolder = System.IO.Path.GetDirectoryName(sourcePdfPath);
                reader = new PdfReader(sourcePdfPath);
                // int page = (reader.NumberOfPages / count);
                // 计算按固定页数生成文档的数量 只要有小数都加1
                int count = (int)Math.Ceiling((double)(reader.NumberOfPages) / (double)(page));
                int startPage = 1;
                int endPage = 1;

                LogUtil.WriteLog("文档数量：" + count.ToString());

                for (int i = 1; i <= count; i++)
                {
                    string outputPdfPath = outputPdfFolder + "\\" + fileNameWithoutExtension + "_" + i + ".pdf"; ;

                    if (i == 1)
                    {
                        startPage = 1;
                        endPage = page;

                    }
                    else
                    {

                        startPage = endPage + 1;
                        endPage = endPage + page;
                    }

                    if (startPage > reader.NumberOfPages)
                        break;

                    if (endPage > reader.NumberOfPages)
                        endPage = reader.NumberOfPages;

                    if (i == count)
                        endPage = reader.NumberOfPages;

                    LogUtil.WriteLog(outputPdfPath + " > " + startPage.ToString() + "-" + endPage.ToString());
                    SplitPDF(sourcePdfPath, outputPdfPath, startPage, endPage);

                }

                reader.Close();
            }
            catch (Exception ex) { throw ex; }
        }


        /// <summary> 
        /// 从已有PDF文件中拷贝指定的页码范围到一个新的pdf文件中 使用pdfCopyProvider.AddPage()方法
        /// </summary>
        /// <param name="sourcePdfPath">文件路径+文件名</param>
        /// <param name="custpages">自定义的页数范围</param>
        public void SplitPDFCustPage(string sourcePdfPath, string custpages)
        {
            //  string[] strArray = custpages.Trim().Split(",");
            string[] strArray = custpages.Trim().Split(new Char[] { ',' });
            string fileNameWithoutExtension = System.IO.Path.GetFileNameWithoutExtension(sourcePdfPath);
            string outputPdfFolder = System.IO.Path.GetDirectoryName(sourcePdfPath);
            int startPage;
            int endPage;

            for (int i = 0; i < strArray.Length; i++)
            {

                LogUtil.WriteLog("自定义页面范围：" + strArray[i]);

                // 横杠-相连的页码，抽取连续的范围内的页码生成到一个文档
                if (strArray[i].Contains("-"))
                {
                  //  string[] array = strArray[i].Split("-");
                    string[] array = strArray[i].Split(new Char[] { '-' });
                    startPage = int.Parse(array[0]);
                    endPage = int.Parse(array[1]);
                    string outputPdfPath = outputPdfFolder + "\\" + fileNameWithoutExtension + " " + startPage + "-" + endPage + ".pdf";
                    LogUtil.WriteLog(outputPdfPath);
                    SplitPDF(sourcePdfPath, outputPdfPath, startPage, endPage);

                }
                // and &相连的页码，抽取指定页码生成到一个文档
                else if (strArray[i].Contains("&"))
                {
                 //   int[] intArray = Array.ConvertAll(strArray[i].Split("&"), int.Parse);
                    int[] intArray = Array.ConvertAll(strArray[i].Split(new Char[] { '&' }), int.Parse);
                    string pages = string.Join("&", intArray);
                    string outputPdfPath = outputPdfFolder + "\\" + fileNameWithoutExtension + " " + pages + ".pdf";
                    LogUtil.WriteLog(outputPdfPath);
                    SplitPDF2ExtractPages(sourcePdfPath, outputPdfPath, intArray);

                }
                else
                {
                    startPage = int.Parse(strArray[i]);
                    endPage = int.Parse(strArray[i]);
                    string outputPdfPath = outputPdfFolder + "\\" + fileNameWithoutExtension + " " + strArray[i] + ".pdf"; ;
                    LogUtil.WriteLog(outputPdfPath);
                    SplitPDF(sourcePdfPath, outputPdfPath, startPage, endPage);
                }
            }
        }

        /// <summary> 
        /// 将已有pdf文件中 不连续 的页拷贝至新的pdf文件中。其中需要拷贝的页码存于数组 int[] extractThesePages中
        /// </summary>
        /// <param name="sourcePdfPath">文件路径+文件名</param>
        /// <param name="extractThesePages">页码集合</param>
        /// <param name="outputPdfPath">文件路径+文件名</param>
        public void SplitPDF2ExtractPages(string sourcePdfPath, string outputPdfPath, int[] extractThesePages)
        {
            PdfReader reader = null;
            Document sourceDocument = null;
            PdfCopy pdfCopyProvider = null;
            PdfImportedPage importedPage = null;

            try
            {
                reader = new PdfReader(sourcePdfPath);
                sourceDocument = new Document(reader.GetPageSizeWithRotation(extractThesePages[0]));
                pdfCopyProvider = new PdfCopy(sourceDocument, new System.IO.FileStream(outputPdfPath, System.IO.FileMode.Create));
                sourceDocument.Open();
                foreach (int pageNumber in extractThesePages)
                {
                    importedPage = pdfCopyProvider.GetImportedPage(reader, pageNumber); pdfCopyProvider.AddPage(importedPage);
                }
                sourceDocument.Close();
                reader.Close();
            }
            catch (Exception ex)
            {
                throw ex;
            }
        }

    }
}
