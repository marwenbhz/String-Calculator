public class StringCalculator{

    public static String add(String n){
        if(n.length() == 0) return "0";
        Double result = 0.0;
        String resultMsg = "";
        String delimiter = ",";
        int start = 0;
        String[] lines = n.split("\n");
        if(lines[0].startsWith("//")){
            delimiter = lines[0].replace("//","");
            start = 1;
        }
        int index = 0;
        for(int i = start; i < lines.length; i++){
            String[] numbers = lines[i].split(getSpecialCharacter(delimiter),-1);
            for(int j = 0; j < numbers.length; j++){
                if(isNumeric(numbers[j])){
                    if(isNegative(numbers[j])){
                        if(!resultMsg.equals("")) resultMsg += "\\n"; 
                        resultMsg += "Negative not allowed : " + numbers[j];
                    }else{
                        result += Double.parseDouble(numbers[j]);
                    }
                }else{
                    if(!resultMsg.equals("")) resultMsg += "\\n"; 
                    if("".equals(numbers[j])){
                        if((i == (lines.length -1)) && (j == (numbers.length -1))){
                            resultMsg += "Number expected but EOF found";
                        }else if((j == (numbers.length -1))){
                            resultMsg += "Number expected but '\\n' found at position " + index;
                        }else{
                            resultMsg += "Number expected but "+ delimiter +" found at position " + index;
                        }
                    }else{
                        resultMsg += delimiter + " expected but "+ getIndex(numbers[j]) +" found at position " + (index + numbers[j].indexOf(getIndex(numbers[j])));
                    }
                }
                index += (numbers[j].length() + delimiter.length());
            }
        }
        if(!resultMsg.equals("")) return resultMsg;
        return String.valueOf(result);
    }
    
    public static boolean isNumeric(String s) {  
        return s != null && s.matches("[-+]?\\d*\\.?\\d+");  
    } 
    public static boolean isNegative(String s) {  
        return Double.parseDouble(s) < 0.0;  
    } 
    public static char getIndex(String s){
        for(int i = 0; i < s.length(); i++){
            if(!Character.isDigit(s.charAt(i))){
                return s.charAt(i);
            }
        }
        return s.charAt(0);
    }
    public static String getSpecialCharacter(String s) {
        String specialChars = "/*!@#$%^&*()\"{}_[]|\\?/<>.";
        if (specialChars.contains(s)) {
            return "\\".concat(s);
        }
        return s;
     }
    public static void main(String []args){
        System.out.println(add(""));
        System.out.println(add("1\n2,3"));
        System.out.println(add("175.2,\n35"));
        System.out.println(add("1,3,"));
        System.out.println(add("//;\n1;2"));
        System.out.println(add("//|\n1|2|3"));
        System.out.println(add("//sep\n2sep3"));
        System.out.println(add("//|\n1|2,3"));
        System.out.println(add("-1,2"));
        System.out.println(add("2,-4,-5"));
        System.out.println(add("-1,,2"));
    }
}