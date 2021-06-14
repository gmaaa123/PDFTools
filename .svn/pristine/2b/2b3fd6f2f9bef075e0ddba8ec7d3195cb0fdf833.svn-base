using System;
using System.Collections.Generic;
using System.Text;
using System.Text.RegularExpressions;

namespace PDFTools
{
    public class StringUtil
    {
        ///<summary>
        /// 移除前缀字符串
        ///</summary>
        ///<param name="val">原字符串</param>
        ///<param name="str">前缀字符串</param>
        ///<returns></returns>
        public string GetRemovePrefixString(string val, string str)
        {

            string strRegex = @"^(" + str + ")";

            return Regex.Replace(val, strRegex, "");

        }

        ///<summary>
        /// 移除后缀字符串
        ///</summary>
        ///<param name="val">原字符串</param>
        ///<param name="str">后缀字符串</param>
        ///<returns></returns>
        public string GetRemoveSuffixString(string val, string str)
        {
            string strRegex = @"(" + str + ")" + "$";

            return Regex.Replace(val, strRegex, "");
        }

        ///<summary>
        /// 移除前后字符串
        ///</summary>
        ///<param name="val">原字符串</param>
        ///<param name="str">要截掉的字符串</param>
        ///<param name="bAllStr">是否对整个字符串进行截取
        ///如果为true则对整个字符串中匹配的进行截取
        ///如果为false则只截取前缀或者后缀</param>
        ///<returns></returns>
        public static string GetString(string val, string str, bool bAllStr)
        {
            return Regex.Replace(val, @"(^(" + str + ")" + (bAllStr ? "*" : "") + "|(" + str + ")" + (bAllStr ? "*" : "") + "$)", "");
        }
    }
}
