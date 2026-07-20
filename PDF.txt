import os
from pypdf import PdfMerger, PdfReader, PdfWriter

def merge_pdfs(pdf_list, output_filename):
    """Combines multiple PDF files into a single output PDF file"""
    if not pdf_list:
        print("\n❌ Error: No PDF files selected for merging.")
        return

    merger = PdfMerger()
    try:
        for pdf in pdf_list:
            if os.path.exists(pdf):
                merger.append(pdf)
                print(f"➕ Added: {pdf}")
            else:
                print(f"⚠️ Warning: File '{pdf}' not found. Skipping...")
        
        merger.write(output_filename)
        merger.close()
        print(f"\n✅ Success! Merged PDF saved as: {output_filename}\n")
    except Exception as e:
        print(f"❌ Error during merge operation: {e}")

def split_pdf(input_pdf, output_dir="split_pages"):
    """Splits a single multi-page PDF into separate single-page PDF files"""
    if not os.path.exists(input_pdf):
        print(f"\n❌ Error: File '{input_pdf}' not found.")
        return

    try:
        reader = PdfReader(input_pdf)
        total_pages = len(reader)
        
        if not os.path.exists(output_dir):
            os.makedirs(output_dir)

        print(f"\nSplitting '{input_pdf}' ({total_pages} pages)...")
        for page_num in range(total_pages):
            writer = PdfWriter()
            writer.add_page(reader.pages[page_num])
            
            output_filename = os.path.join(output_dir, f"page_{page_num + 1}.pdf")
            with open(output_filename, "wb") as f:
                writer.write(f)
            print(f"💾 Saved: {output_filename}")
            
        print(f"\n✅ Success! All pages saved individually inside the '{output_dir}' directory.\n")
    except Exception as e:
        print(f"❌ Error during split operation: {e}")

def main():
    print("--- Welcome to PDF Merger & Splitter Utility ---")
    while True:
        print("1. Merge PDFs")
        print("2. Split PDF")
        print("3. Exit")
        
        choice = input("Select an option (1/2/3): ").strip()
        
        if choice == '1':
            print("\n--- PDF Merger ---")
            print("Enter file names separated by spaces (e.g., file1.pdf file2.pdf)")
            files_input = input("Enter PDF file names: ").strip()
            pdf_list = files_input.split()
            
            if pdf_list:
                output_name = input("Enter output filename (e.g., merged.pdf): ").strip()
                if not output_name.endswith('.pdf'):
                    output_name += '.pdf'
                merge_pdfs(pdf_list, output_name)
            else:
                print("❌ Error: No file names provided.")
                
        elif choice == '2':
            print("\n--- PDF Splitter ---")
            input_pdf = input("Enter the PDF filename to split (e.g., document.pdf): ").strip()
            split_pdf(input_pdf)
            
        elif choice == '3':
            print("Thank you for using PDF Utility. Goodbye!")
            break
        else:
            print("\n❌ Invalid choice! Please select 1, 2, or 3.\n")

if __name__ == "__main__":
    main()
