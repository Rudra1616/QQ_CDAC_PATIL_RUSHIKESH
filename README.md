# QQ_CDAC_PATIL_RUSHIKESH
Algorithms and Programs for translating the below PDF text from Marathi To English.

import java.io.File;
import java.io.IOException;
import java.nio.file.Files;
import com.google.cloud.translate.Translate;
import com.google.cloud.translate.TranslateOptions;
import com.google.cloud.translate.Translation;
import org.apache.pdfbox.pdmodel.PDDocument;
import org.apache.pdfbox.text.PDFTextStripper;

public class PdfTranslator {

    public static void main(String[] args) {
        String inputPdfPath = "D:\MaraToEnga"; 
        String outputPdfPath = "D:\ResultMaraToEnga"; 
        try {
            String marathiText = extractTextFromPdf(inputPdfPath);
            String translatedText = translateText(marathiText, "mr", "en");
            writeTextToPdf(translatedText, outputPdfPath);
            System.out.println("Translation complete. Translated text saved to " + outputPdfPath);
        } catch (IOException e) {
            System.err.println("Error: " + e.getMessage());
        }
    }

    private static String extractTextFromPdf(String pdfFilePath) throws IOException {
        try (PDDocument document = PDDocument.load(new File(pdfFilePath))) {
            PDFTextStripper pdfStripper = new PDFTextStripper();
            return pdfStripper.getText(document);
        }
    }

    private static String translateText(String text, String sourceLanguage, String targetLanguage) {
        Translate translate = TranslateOptions.getDefaultInstance().getService();
        Translation translation = translate.translate(text, Translate.TranslateOption.sourceLanguage(sourceLanguage),
                Translate.TranslateOption.targetLanguage(targetLanguage));
        return translation.getTranslatedText();
    }

    private static void writeTextToPdf(String text, String pdfFilePath) throws IOException {
        Files.write(new File(pdfFilePath).toPath(), text.getBytes());
    }
}

