import StudentsPicker from '../components/StudentsPicker';
import StudentsTable from '../components/StudentsTable';
import { fetchStudentData, fetchSchoolData, fetchLegalguardianData } from '../utils';
import { useState } from 'react';


const studentsDataComponent = () => {
    const [studentsData, setStudentsData] = useState([]);
    const [schoolsData, setSchoolsData] = useState([]);
    const [legalguardiansData, setLegalguardiansData] = useState([]);

    const onStudentsPick = async (studentIds) => {
        const studentsData = [];
        const schoolsData = [];
        const legalguardiansData = [];

        for (const studentId of studentIds) {
            const studentData = await fetchStudentData(studentId);
            studentsData.push(studentData);

            const { schoolId, legalguardianId } = studentData;

            const [schoolData, legalguardianData] = await Promise.all([
                fetchSchoolData(schoolId),
                fetchLegalguardianData(legalguardianId),
            ]);

            schoolsData.push(schoolData);
            legalguardiansData.push(legalguardianData);
        }

        setStudentsData([...studentsData]);
        setSchoolsData([...schoolsData]);
        setLegalguardiansData([...legalguardiansData]);
    };



    return (
        <>
            <StudentsPicker onPickHandler={onStudentsPick} />
            <StudentsTable
                studentsData={studentsData}
                schoolsData={schoolsData}
                LegalguardiansData={legalguardiansData}
            />
        </>
    );
};


export default studentsDataComponent;
