const sampleData = `Introduction to Machine Learning

Machine learning is a subset of artificial intelligence that enables computers to learn and improve from experience without being explicitly programmed. It focuses on developing algorithms that can automatically learn patterns from data.

Types of Machine Learning:

1. Supervised Learning
Supervised learning uses labeled datasets to train algorithms. The algorithm learns from input-output pairs and can make predictions on new, unseen data. Common examples include classification (predicting categories) and regression (predicting continuous values).

2. Unsupervised Learning
Unsupervised learning finds hidden patterns in data without labeled examples. It includes techniques like clustering (grouping similar data points) and dimensionality reduction (simplifying data while preserving important information).

3. Reinforcement Learning
Reinforcement learning involves an agent learning to make decisions by interacting with an environment. The agent receives rewards or penalties based on its actions and learns to maximize cumulative rewards over time.

Key Concepts:
- Training data: Dataset used to teach the algorithm
- Features: Individual measurable properties of observed phenomena
- Model: Mathematical representation of a real-world process
- Overfitting: When a model learns training data too well and fails on new data
- Cross-validation: Technique to assess model performance on unseen data

Applications:
Machine learning is used in various fields including image recognition, natural language processing, recommendation systems, autonomous vehicles, and medical diagnosis.`;

        // Text processing utilities
        class TextProcessor {
            static preprocessText(text) {
                // Clean up text
                return text
                    .replace(/\s+/g, ' ')
                    .replace(/[^\w\s\.\?\!\-\:]/g, '')
                    .trim();
            }

            static extractSentences(text) {
                return text.split(/[.!?]+/).filter(s => s.trim().length > 10);
            }

            static extractKeywords(text) {
                const stopWords = new Set(['the', 'is', 'at', 'which', 'on', 'a', 'an', 'and', 'or', 'but', 'in', 'with', 'to', 'for', 'of', 'as', 'by', 'that', 'this', 'it', 'from', 'they', 'we', 'you', 'he', 'she', 'be', 'have', 'do', 'say', 'get', 'make', 'go', 'know', 'take', 'see', 'come', 'think', 'look', 'want', 'give', 'use', 'find', 'tell', 'ask', 'work', 'seem', 'feel', 'try', 'leave', 'call']);
                
                const words = text.toLowerCase().match(/\b[a-z]{3,}\b/g) || [];
                const wordFreq = {};
                
                words.forEach(word => {
                    if (!stopWords.has(word)) {
                        wordFreq[word] = (wordFreq[word] || 0) + 1;
                    }
                });
                
                return Object.entries(wordFreq)
                    .sort((a, b) => b[1] - a[1])
                    .slice(0, 10)
                    .map(([word, freq]) => ({ word, frequency: freq }));
            }

            static extractNamedEntities(text) {
                // Simple NER simulation
                const patterns = [
                    /\b[A-Z][a-z]+ [A-Z][a-z]+\b/g, // Person names
                    /\b[A-Z][a-z]*(?:\s+[A-Z][a-z]*)*\s+(?:University|College|School|Institute)\b/g, // Institutions
                    /\b(?:January|February|March|April|May|June|July|August|September|October|November|December)\s+\d{1,2},?\s+\d{4}\b/g, // Dates
                    /\b[A-Z][a-z]*(?:\s+[A-Z][a-z]*)*\s+(?:Algorithm|Method|Theory|Model|System)\b/g // Technical terms
                ];
                
                const entities = [];
                patterns.forEach(pattern => {
                    const matches = text.match(pattern) || [];
                    entities.push(...matches);
                });
                
                return [...new Set(entities)];
            }
        }

        // Summarization engine
        class Summarizer {
            static extractiveSummary(text, length = 'medium') {
                const sentences = TextProcessor.extractSentences(text);
                const keywords = TextProcessor.extractKeywords(text);
                const keywordSet = new Set(keywords.map(k => k.word));
                
                // Score sentences based on keyword frequency
                const sentenceScores = sentences.map(sentence => {
                    const words = sentence.toLowerCase().match(/\b[a-z]+\b/g) || [];
                    const score = words.reduce((sum, word) => {
                        return sum + (keywordSet.has(word) ? 1 : 0);
                    }, 0);
                    return { sentence: sentence.trim(), score };
                });
                
                // Sort by score and select top sentences
                sentenceScores.sort((a, b) => b.score - a.score);
                
                let numSentences;
                switch(length) {
                    case 'short': numSentences = 3; break;
                    case 'long': numSentences = 8; break;
                    default: numSentences = 5;
                }
                
                return sentenceScores
                    .slice(0, Math.min(numSentences, sentences.length))
                    .map(s => s.sentence)
                    .join('. ') + '.';
            }
        }

        // Structure extractor
        class StructureExtractor {
            static extractStructure(text) {
                const lines = text.split('\n').filter(line => line.trim());
                const structure = { title: '', sections: [] };
                let currentSection = null;
                
                lines.forEach(line => {
                    line = line.trim();
                    
                    // Check if it's a title (first non-empty line or all caps)
                    if (!structure.title && line.length > 0) {
                        structure.title = line;
                        return;
                    }
                    
                    // Check if it's a section header
                    if (line.match(/^\d+\.\s+|^[A-Z][a-z\s]+:$|^#+\s+/)) {
                        currentSection = {
                            title: line.replace(/^\d+\.\s+|^#+\s+/, '').replace(/:$/, ''),
                            content: []
                        };
                        structure.sections.push(currentSection);
                    } else if (currentSection) {
                        // Add content to current section
                        if (line.length > 0) {
                            currentSection.content.push(line);
                        }
                    } else {
                        // No section yet, create a general section
                        if (structure.sections.length === 0) {
                            structure.sections.push({
                                title: 'Overview',
                                content: []
                            });
                        }
                        if (line.length > 0) {
                            structure.sections[structure.sections.length - 1].content.push(line);
                        }
                    }
                });
                
                return structure;
            }
        }

        // Quiz generator
        class QuizGenerator {
            static generateQuestions(text, count = 5) {
                const sentences = TextProcessor.extractSentences(text);
                const keywords = TextProcessor.extractKeywords(text);
                const questions = [];
                
                // Generate different types of questions
                const questionTypes = ['definition', 'multiple_choice', 'true_false', 'fill_blank'];
                
                for (let i = 0; i < Math.min(count, sentences.length); i++) {
                    const sentence = sentences[i];
                    const type = questionTypes[i % questionTypes.length];
                    
                    switch(type) {
                        case 'definition':
                            if (keywords.length > i) {
                                questions.push({
                                    type: 'short_answer',
                                    question: `What is ${keywords[i].word}?`,
                                    answer: sentence
                                });
                            }
                            break;
                            
                        case 'multiple_choice':
                            const mcq = this.generateMCQ(sentence, keywords);
                            if (mcq) questions.push(mcq);
                            break;
                            
                        case 'true_false':
                            questions.push({
                                type: 'true_false',
                                question: `True or False: ${sentence}`,
                                answer: 'True'
                            });
                            break;
                            
                        case 'fill_blank':
                            const fillBlank = this.generateFillBlank(sentence, keywords);
                            if (fillBlank) questions.push(fillBlank);
                            break;
                    }
                }
                
                return questions.slice(0, count);
            }
            
            static generateMCQ(sentence, keywords) {
                if (keywords.length < 3) return null;
                
                const correctAnswer = keywords[0].word;
                const wrongAnswers = keywords.slice(1, 4).map(k => k.word);
                
                return {
                    type: 'multiple_choice',
                    question: `Which concept is most relevant to: "${sentence.substring(0, 80)}..."?`,
                    options: [correctAnswer, ...wrongAnswers].sort(() => Math.random() - 0.5),
                    answer: correctAnswer
                };
            }
            
            static generateFillBlank(sentence, keywords) {
                if (keywords.length === 0) return null;
                
                const keyword = keywords[0].word;
                const regex = new RegExp(`\\b${keyword}\\b`, 'gi');
                const questionText = sentence.replace(regex, '_____');
                
                if (questionText === sentence) return null;
                
                return {
                    type: 'fill_blank',
                    question: `Fill in the blank: ${questionText}`,
                    answer: keyword
                };
            }
        }

        // Main processing function
        function processNotes() {
            const textInput = document.getElementById('textInput').value.trim();
            
            if (!textInput) {
                alert('Please enter some text to process!');
                return;
            }
            
            // Show loading
            document.getElementById('loading').classList.remove('hidden');
            document.getElementById('results').classList.add('hidden');
            
            // Simulate processing delay
            setTimeout(() => {
                const summaryLength = document.getElementById('summaryLength').value;
                const quizCount = parseInt(document.getElementById('quizCount').value);
                
                // Process text
                const cleanText = TextProcessor.preprocessText(textInput);
                const summary = Summarizer.extractiveSummary(cleanText, summaryLength);
                const structure = StructureExtractor.extractStructure(textInput);
                const keywords = TextProcessor.extractKeywords(cleanText);
                const entities = TextProcessor.extractNamedEntities(cleanText);
                const quiz = QuizGenerator.generateQuestions(cleanText, quizCount);
                
                // Update statistics
                const wordCount = cleanText.split(/\s+/).length;
                const sentenceCount = TextProcessor.extractSentences(cleanText).length;
                
                document.getElementById('wordCount').textContent = wordCount;
                document.getElementById('sentenceCount').textContent = sentenceCount;
                document.getElementById('conceptCount').textContent = keywords.length + entities.length;
                document.getElementById('questionCount').textContent = quiz.length;
                
                // Display results
                displaySummary(summary);
                displayStructure(structure);
                displayKeyConcepts(keywords, entities);
                displayQuiz(quiz);
                
                // Show results
                document.getElementById('loading').classList.add('hidden');
                document.getElementById('results').classList.remove('hidden');
                
                // Scroll to results
                document.getElementById('results').scrollIntoView({ behavior: 'smooth' });
            }, 1500);
        }

        function displaySummary(summary) {
            document.getElementById('summaryContent').innerHTML = `<p>${summary}</p>`;
        }

        function displayStructure(structure) {
            let html = '';
            
            if (structure.title) {
                html += `<h4>${structure.title}</h4>`;
            }
            
            structure.sections.forEach(section => {
                html += `<h4>${section.title}</h4>`;
                html += '<ul>';
                section.content.forEach(item => {
                    html += `<li>${item}</li>`;
                });
                html += '</ul>';
            });
            
            document.getElementById('structuredContent').innerHTML = html;
        }

        function displayKeyConcepts(keywords, entities) {
            const container = document.getElementById('keyConcepts');
            container.innerHTML = '';
            
            keywords.forEach(keyword => {
                const tag = document.createElement('span');
                tag.className = 'concept-tag';
                tag.textContent = keyword.word;
                container.appendChild(tag);
            });
            
            entities.forEach(entity => {
                const tag = document.createElement('span');
                tag.className = 'concept-tag';
                tag.textContent = entity;
                container.appendChild(tag);
            });
        }

        function displayQuiz(questions) {
            const container = document.getElementById('quizQuestions');
            container.innerHTML = '';
            
            questions.forEach((q, index) => {
                const questionDiv = document.createElement('div');
                questionDiv.className = 'question';
                
                let html = `<h5>Question ${index + 1}: ${q.question}</h5>`;
                
                if (q.type === 'multiple_choice') {
                    html += '<div class="question-options">';
                    q.options.forEach((option, i) => {
                        html += `<div>${String.fromCharCode(65 + i)}. ${option}</div>`;
                    });
                    html += '</div>';
                }
                
                html += `<div class="answer">Answer: ${q.answer}</div>`;
                
                questionDiv.innerHTML = html;
                container.appendChild(questionDiv);
            });
        }

        function clearContent() {
            document.getElementById('textInput').value = '';
            document.getElementById('results').classList.add('hidden');
        }

        function loadSampleData() {
            document.getElementById('textInput').value = sampleData;
        }

        // File upload handler
        document.getElementById('fileInput').addEventListener('change', function(event) {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    if (file.type.startsWith('text/')) {
                        document.getElementById('textInput').value = e.target.result;
                    } else {
                        alert('Audio file processing would require server-side implementation with Whisper API');
                    }
                };
                reader.readAsText(file);
            }
        });

        // Initialize with sample data
        window.onload = function() {
            loadSampleData();
        };
